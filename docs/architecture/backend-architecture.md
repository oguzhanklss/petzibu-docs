# Petzibu – Backend Mimari Dokümanı

> Kapsam: `apps/api` (NestJS + Prisma + PostgreSQL) ve monorepo düzeni.
> İlgili: [Epic'ler](../epics/index.md) · [Backend kuralları](../rules-backend.md) · [Mobil kuralları](../rules-mobile.md) · [ADR'ler](decisions/index.md)

Bu belge backend'in nasıl kurulduğunu ve hangi kurallarla büyüyeceğini tanımlar. Ürün kararları (K1–K58) epic dosyalarında yaşar; burada yalnızca mimariyi etkileyenlere atıf yapılır.

---

## 1. Genel yaklaşım: modüler monolit

Petzibu **tek bir NestJS uygulaması** olarak çalışır. Tek süreç, tek PostgreSQL veritabanı, tek deploy. İçeride kod, sınırları kurallarla korunan modüllere bölünür.

**Neden mikroservis değil**

- Mikroservisin asıl faydası, ayrı ekiplerin bağımsız deploy etmesidir. Petzibu'da bu ihtiyaç yok; maliyetleri (servisler arası iletişim, dağıtık transaction, ayrı izleme) ise baştan gelir.
- Domain sıkı bağlıdır. Randevu tamamlanınca döküm oluşur; tahsilat dökümlere dağıtılır (K41); müşteri silme borca ve ileri tarihli randevuya bakar (K54). Bunlar tek Postgres transaction'ında basittir, servislere bölünürse saga gerektirir.
**Geleceğe hazırlık servislerle değil, sınırlarla yapılır.** Modüller birbirinin tablosuna dokunmaz ve yalnızca birbirinin servisini çağırır (§4.2). Bir modülün ayrı servis olması gerekirse sınır zaten hazırdır: servis çağrısı HTTP veya mesaj çağrısına, ortak transaction ise event'e dönüşür.

**Bir modül ancak şu durumlarda ayrı servis olur:** bağımsız ölçeklenmesi gereken ölçülmüş bir yük, ayrı bir ekibin sahipliği ya da farklı bir güvenlik veya uyum sınırı. Bunlardan biri ortaya çıkmadan ayırma yapılmaz.

---

## 2. Teknoloji yığını

Yığının kanonik listesi [backend kurallarında](../rules-backend.md#stack) durur; burada tekrar edilmez. Bu doküman yalnızca **mimari gerekçesi olan** seçimleri kendi bölümlerinde anlatır: monorepo düzeni ve `packages/contracts`'ın paylaşımı §3, Prisma şemasının modül başına bölünmesi §5.1, tenant ve transaction bağlamı (nestjs-cls) §5.2 ve §5.3, Zod tabanlı API sözleşmesi ve Swagger üretimi §6 (hata modeli ve i18n'in neden olmadığı §6.3), argon2id §7.1, BullMQ ile arka plan işleri §8, S3 dosyaları ile döküm PDF'i ve dışa aktarma Excel'i §9, Handlebars ile public web §10, env doğrulaması §11.2.

---

## 3. Monorepo yapısı

```text
apps/
  api/            # NestJS; public web sayfaları dahil (K14)
  mobile/         # Expo uygulaması (eski `petzibu` reposundan taşındı)
  web/            # React back office (K47) — web'e başlanınca açılır
packages/
  contracts/      # API sözleşmesi: zod şemaları, enum'lar, hata kodları, envelope
  tsconfig/
  eslint-config/
```

- **Yalnızca sözleşme paylaşılır.** UI bileşenleri mobil ile web arasında paylaşılmaz.
- **Landing site** API'ye bağımlı değildir; yeri (monorepo'da `apps/landing` ya da harici bir araç) pilot bitince belirlenir.
- Mobil, Expo'nun monorepo dokümanına göre yapılandırılmıştır (Metro + pnpm, kökte `node-linker=hoisted`). `expo/metro-config` `pnpm-workspace.yaml`'ı okuyup `watchFolders`'ı kendisi kurar; `metro.config.js`'te monorepo'ya özel bir ayar yoktur.
- **`contracts` istemcilere `dist`'ten gider.** Paket `moduleResolution: nodenext` kullandığı için göreli import'ları `./x.js` yazmak zorunda; Metro ve jest bu yolu `.ts` kaynağına eşlemez. Bu yüzden `exports` derlenmiş çıktıyı gösterir ve sırayı Turborepo kurar (`type-check`, `test`, `dev`, `start` → `^build`).

---

## 4. Modüller

### 4.1. Modül haritası

```text
apps/api/src/
├── common/              # filters, guards, interceptors, logger, queue, swagger, utils
├── config/
├── database/            # PrismaService + tenant extension
├── health/
└── modules/
    │  # Platform — domain bilmez
    ├── mail/
    ├── files/                 # S3 presigned upload/download
    ├── document-generator/    # PDF + Excel adapter'ları
    │
    │  # Çekirdek
    ├── users/                 # owner ve admin kullanıcıları
    ├── auth/                  # giriş, refresh, şifre sıfırlama, davet kabulü
    ├── businesses/            # işletme profili + durum (K34, K57)
    ├── business-hours/        # çalışma saatleri + özel kapalı günler
    ├── legal/                 # metin sürümleri + onaylar (K58)
    │
    │  # Domain
    ├── catalog/               # hizmetler, kademeler, ek ücret kalemleri
    ├── customers/
    ├── pets/                  # hayvan, aşı, uyarı etiketleri, fotoğraflar, özel fiyat
    ├── appointments/          # randevu, satırlar, durum geçişleri, uyarılar, fiyat kuralı (K32), onay token'ı
    ├── grooming-reports/      # bakım raporu, önce/sonra fotoğrafları, sonraki bakım önerisi (K40)
    ├── billing/               # döküm, indirim, tahsilat, borç dağıtımı (K41)
    ├── expenses/
    │
    │  # Orkestrasyon — birden çok modülü birleştirir, kimse bunlara bağımlı değildir
    ├── reminders/             # kuyruk sorgusu, şablonlar, gönderildi kaydı, rebook
    ├── intake/                # linkler, başvurular, onay → customers/pets
    ├── reports/               # kasa ve aylık özet (salt okuma)
    ├── privacy/               # müşteri anonimleştirme, dışa aktarma, hesap silme
    ├── admin/                 # iç araç: işletme aç, davet et, durum, metin yayınla
    └── public-web/            # SSR sayfalar: davet, sıfırlama, intake formu, onay linki
```

**Bölme kuralı**

- Bir modül **kendi tablolarının tek sahibidir**.
- İki şey **ayrı sebeplerle değişiyorsa** ayrı modül olur.
- İki şey **aynı kuralı birlikte koruyorsa** bölünmez. Randevu ve satırları bir aradadır (süre satırların toplamı, K30); döküm ve tahsilat bir aradadır (K41).
- Tek bir kural için modül açılmaz. Fiyat kuralı (K32) bugün yalnızca randevuda kullanıldığı için `appointments`'ta durur, özel fiyat tablosu `pets`'te. İkinci bir kullanıcı (ör. lodging) gelince ayrılır.
### 4.2. Modül anayasası

1. **Tablo sahipliği.** Bir modül başka bir modülün tablolarını Prisma ile okumaz ve yazmaz. Veri, sahibi olan modülün servisinden istenir. Şemada modüller arası foreign key serbesttir; kısıt koddaki erişimdir.
2. **Tek yönlü bağımlılık.** Döngü yoktur. Nest'te `forwardRef` kullanılmaz; ihtiyaç duyuluyorsa mantık yanlış modüldedir ve bir üst katmana (genellikle orkestrasyon) taşınır.
3. **Yalnızca doğrudan servis çağrısı.** In-process event bus kullanılmaz. Modül sınırını aşan yan etki (ör. "randevu tamamlandı → döküm oluştur") çağıran serviste açıkça yazılır.
4. **Public yüzey.** Bir modül yalnızca `exports`'ta listelediği servisleri dışarı açar. Controller'lar başka modülün servisini değil, kendi servisini çağırır.
5. **`any` yasak.** Modüller arası veri, `contracts`'taki tiplerle ya da servisin açık dönüş tipiyle taşınır.
**Bağımlılık yönü**

```text
platform ← çekirdek ← catalog, customers
                       customers ← pets
                       catalog, pets, business-hours ← appointments ← grooming-reports
                                                       appointments → billing
orkestrasyon (reminders, intake, reports, privacy, admin, public-web) → herkes
```

`privacy` bu kuralın örneğidir: müşteri silme borca (`billing`) ve ileri tarihli randevuya (`appointments`) bakar. Bu mantık `customers`'ta olsaydı `customers → billing → customers` döngüsü oluşurdu.

### 4.3. Modül içi yapı

```text
appointments/
├── appointments.module.ts
├── appointments.controller.ts
├── appointments.service.ts
└── appointments.service.spec.ts
```

- **Servis + doğrudan Prisma.** Repository katmanı ve CQRS kullanılmaz. Boilerplate'teki `announcements` (CQRS) ve `users` (repository) örnekleri yeni modüllere model değildir.
- DTO sınıfı yazılmaz; şemalar `contracts`'tan gelir (§6).
- Bir modül gerçekten büyüdüğünde klasörlere ayrılır, önceden değil.
### 4.4. Bildirim kanalları

MVP'de tek giden kanal **e-postadır** (`mail`): davet, şifre sıfırlama ve askı uyarısı. Push ve SMS modülü yoktur.

Boilerplate'in `notifications` (Firebase push) ve `sms` (FONIVA) modülleri bir süre "uykuda" tutulacaktı; geçişte silindiler, çünkü ikisi de i18n ve RBAC'a bağlıydı ve Petzibu'nun push ile SMS ihtiyacı henüz hiçbir epic'te tanımlı değil. Otomatik WhatsApp ve push (Faz 2) tasarlanırken kanal o günün ihtiyacına göre sıfırdan yazılır; hatırlatma gönderimi zaten `reminders` modülünün BullMQ işinden geçer (§8).

---

## 5. Veri katmanı

### 5.1. Prisma şeması

- `apps/api/prisma/schema/` altında **modül başına bir dosya**: `businesses.prisma`, `appointments.prisma`, `billing.prisma`... Dosyanın sahibi modüldür.
- Tablo adları `snake_case` çoğul (`@@map`), alan adları kodda `camelCase`.
- Migration'lar `prisma migrate` ile yönetilir. Üretimde yalnızca `prisma migrate deploy`.
### 5.2. Tenant izolasyonu

Tenant **işletmedir** (K5). İşletmeye ait her tabloda `businessId` bulunur.

**Mekanizma**

1. Auth guard, token'daki `businessId`'yi request context'ine (CLS) yazar. İstemci `businessId` göndermez (RULES.md).
2. `database/` altındaki **Prisma extension**, tenant tablolarındaki her sorguya `businessId` filtresini ekler; `create`'lerde alanı doldurur.
3. Tenant tablosuna context'siz bir sorgu gelirse extension **hata fırlatır**. Varsayılan davranış yoktur.
4. Tenant tablolarının listesi extension'da tek yerde tutulur.
**Kurallar**

- **Raw SQL** tenant tablolarında yasaktır. Rapor gibi zorunlu yerlerde `businessId` parametresi alan fonksiyonlar tek bir dosyada (`reports/reports.sql.ts`) toplanır.
- **Tenant'sız çalışan kod** (admin iç aracı, zamanlanmış işler) context'i açıkça `runAsSystem()` ile kurar. Tek yol budur. Bir işletme adına çalışan job ise `runAsTenant(businessId)` kullanır.
- **Public web** işletmeyi linkteki token'dan bulur ve context'i `runAsTenant` ile kurar.
- **Her modül için cross-tenant e2e testi zorunludur:** A işletmesinin token'ıyla B işletmesinin kaydına okuma ve yazma denemesi `404` döner. Extension'ı gerçekten doğrulayan şey bu testlerdir.
### 5.3. Transaction

- `@nestjs-cls/transactional` kullanılır. Servisler `tx` parametresi taşımaz; `@Transactional()` ile işaretlenen metot içindeki bütün Prisma çağrıları, başka modüllerin servislerinden gelenler dahil, aynı transaction'da çalışır.
- Transaction adapter'a verilen client, **tenant extension'lı client'tır**. Uygulamada tek bir Prisma client vardır.
- Transaction'ı orkestra eden metot başlatır. Örnek: `appointments.complete()` → `@Transactional()` → durum güncellemesi + `billing.createStatement()`.
### 5.4. Kimlikler

- Birincil anahtarlar **UUID v7** (`@default(uuid(7))`). Sıralanabilir, tahmin edilemez.
- Link token'ları ID değildir; 32 bayt rastgele değer, base64url.
  - **Tek kullanımlık** token'lar (davet, şifre sıfırlama) veritabanında SHA-256 hash olarak saklanır.
  - **Paylaşılan** token'lar (genel ve kişiye özel intake linki K22, onay linki K52) owner'a tekrar gösterildiği için düz saklanır; yeniden üretilince eskisi geçersiz olur.
### 5.5. Para

- **Tam sayı kuruş**: veritabanında `Int`, JSON'da tam sayı. `450,00 ₺` → `45000`. Biçimlendirme yalnızca ekranda yapılır (RULES.md ile aynı).
- **Tek para birimi TRY.** Değerlerle birlikte `currency` alanı taşınmaz.
- Para alanlarında birim eki yoktur (`price`, `total`, `amount`): bütün para alanları kuruştur. Tek istisnasız kural olduğu için ek gerekmez.
- Yüzde indirim (K43) kuruşa çevrilirken **yarım yukarı yuvarlanır**; hesap tek bir fonksiyonda (`billing`) yapılır.
- Fiyatlar KDV dahildir, vergi hesaplanmaz (K45).
- `Int` üst sınırı (~21 milyon TL) tek bir işlem için yeterlidir. Toplamlar Postgres'te `bigint` olarak hesaplanır ve JS'e `number` olarak döner.
### 5.6. Zaman ve süre

| Tür | Postgres | JSON | Örnek | Kullanım |
| --- | --- | --- | --- | --- |
| Zaman anı | `timestamptz` | ISO 8601 UTC, `Z` ile | `2026-09-24T12:30:00.000Z` | Randevu başlangıç/bitiş, `createdAt`, tahsilat, onay |
| Yerel tarih | `date` | `YYYY-MM-DD` | `2026-10-29` | Kapalı gün, aşı tarihi, `validUntil`, gider tarihi |
| Yerel saat | `time` | `HH:mm` | `09:00` | Çalışma saatleri, hatırlatma slotları (K50) |
| Süre | `int` | tam sayı, birim alan adında | `durationMinutes: 90` | Hizmet ve randevu süresi |

- Zaman anları yalnızca `Z` ile kabul edilir; offset'li biçim (`+03:00`) reddedilir.
- Yerel tarih ve saat **asla `Date` nesnesine çevrilmez**; baştan sona string kalır. Prisma'nın `@db.Date` / `@db.Time` alanlarını `Date` olarak döndürmesi tek yerde, serializer'da dönüştürülür.
- İşletmede `timezone` alanı bulunur (varsayılan `Europe/Istanbul`). "Yarın 18:00", "çalışma saati dışı", "15 dakikalık yuvarlama" gibi hesaplar bu saat dilimine göre yapılır.
- Süreler için ISO 8601 süre biçimi (`PT1H30M`) kullanılmaz. Hizmet süreleri 5 dakikalık adımla doğrulanır (`z.int().multipleOf(5)`), rebook aralığı hafta cinsindendir (`rebookIntervalWeeks`).
### 5.7. Silme

Genel bir soft-delete mekanizması yoktur. Boilerplate'ten gelen soft-delete Prisma extension'ı ve tablolardaki `deletedAt`, `createdBy`, `updatedBy` alanları kaldırılmıştır. Her silme türü kendi kuralıyla çalışır:

| Ne | Nasıl | Karar |
| --- | --- | --- |
| Hayvan | `archivedAt` ile arşivlenir | K20 |
| Müşteri | `privacy` modülü anonimleştirir; kişisel veri ve fotoğraflar silinir, randevu ve kasa geçmişi kalır | K54 |
| İşletme | Günlük job kalıcı siler: veritabanında cascade, S3'te `businesses/{businessId}/` öneki | K34, K57 |
| Bekleyen intake formu | 30 gün sonra günlük job siler | INT-04 |

---

## 6. API sözleşmesi

### 6.1. `packages/contracts`

Backend ile istemcilerin tek sözleşme kaynağıdır.

```text
packages/contracts/src/
├── primitives.ts     # id, isoDateTime, localDate, localTime, money, durationMinutes, phone
├── envelope.ts       # envelope(), paginated(), unwrap(), unwrapPaginated(), errorBody, listQuery
├── errors.ts         # ErrorCode enum, ERROR_STATUS, ERROR_MESSAGE
├── enums.ts          # AppointmentStatus, BusinessStatus, Species, SizeTier...
└── <modül>.ts        # auth, users, legal, files, appointments, businesses,
                      # customers, pets, services, extras
```

Bir modülün şeması, API'de karşılığı yazılmadan önce de burada olabilir: sözleşme önce sabitlenir, backend ve istemci ona uyar. Bugün `appointments`'tan `extras`'a kadar olanların yalnızca istemci tarafı vardır.

- **Backend:** controller'lar `createZodDto(schema)` ile doğrular (`ZodValidationPipe` global). Response'lar da şemadan geçer (`ZodSerializerInterceptor`): şemada olmayan alan dışarı çıkmaz, `passwordHash` gibi alanların sızması yapısal olarak imkânsızdır. Swagger aynı şemalardan üretilir.
- **Mobil ve web:** aynı şemalar `lib/api.ts`'te response parse'ı (`unwrap`, `unwrapPaginated`), React Hook Form'da form doğrulaması için kullanılır. Mobilde tele giden şema kalmadı; `features/*/schema.ts` yalnızca ekran-özel türetmeleri (metin olarak yazılan form alanları) tutar.
- **Public web:** intake formu sunucuda aynı şemayla doğrulanır.
- Paket `tsup` ile derlenir (ESM + CJS + `.d.ts`); Turborepo `^build` bağımlılığıyla önce derler.
- Route tanımları pakete girmez; controller'lar Nest'te bildiğin gibi kalır.
- nestjs-zod'un Zod 4 ve güncel `@nestjs/swagger` desteği kurulumda sürüm bazında doğrulanır.
**Enum'ların tek kaynağı.** Kaynak `contracts`'taki enum'dur. Prisma kendi enum'larını üretir ve `contracts` Prisma'ya bağımlı olamaz (mobil import eder). Bu yüzden `apps/api/src/database/enum-parity.ts` içinde iki enum'un birebir aynı olduğu **tip seviyesinde** kontrol edilir; biri değişip diğeri değişmezse `type-check` kırılır.

### 6.2. Envelope

RULES.md'deki biçim aynen korunur; `envelope.ts` bunun tek tanımıdır.

```jsonc
// Başarılı
{ "success": true, "status": 200, "data": { ... }, "message": "..." }

// Liste
{ "success": true, "status": 200, "data": [ ... ],
  "meta": { "page": 1, "limit": 20, "total": 57, "totalPages": 3,
            "hasNextPage": true, "hasPreviousPage": false } }

// Hata
{ "success": false, "status": 422, "code": "APPOINTMENT_WARNINGS",
  "message": "...", "errors": [ { "code": "OVERLAP", "message": "..." } ] }
```

- Takvim gibi tarih aralığı listeleri sayfalanmaz: `dateFrom` + `dateTo`, düz `envelope(z.array(...))`.
- Global response interceptor envelope'u sarar; global exception filter hata gövdesini üretir.
### 6.3. Hata modeli

- Her hata **makine tarafından okunabilir bir `code`** taşır. `message` insan içindir; istemci ona göre dallanmaz.
- Kodlar `contracts/errors.ts`'te tek enum'dur. Domain hataları `DomainException(code, status, errors?)` ile fırlatılır.
- Zod doğrulama hataları `400 VALIDATION_FAILED` olur; `errors[].field` form alanına eşlenir.
| Kod | HTTP | Anlamı |
| --- | --- | --- |
| `VALIDATION_FAILED` | 400 | İstek şemaya uymuyor |
| `UNAUTHORIZED` | 401 | Token yok, geçersiz veya süresi dolmuş |
| `TENANT_SUSPENDED` | 403 | İşletme Askıda veya Silinecek; yazma reddedildi (K34, K57) |
| `CONSENT_REQUIRED` | 403 | Owner yeni metin sürümünü onaylamadı (K58) |
| `NOT_FOUND` | 404 | Kayıt yok ya da başka işletmeye ait |
| `APPOINTMENT_FINALIZED` | 409 | Sonuçlanmış randevu değiştirilemez |
| `APPOINTMENT_WARNINGS` | 422 | Uyarı var, `confirmWarnings: true` bekleniyor (K31) |

Liste epic'ler ilerledikçe `errors.ts`'te büyür; bu tablo örnektir.

---

## 7. Kimlik doğrulama, işletme durumu ve iç araç

### 7.1. Kimlik doğrulama

- E-posta + şifre (K8). Şifreler **argon2id** ile hash'lenir (`argon2` paketi, varsayılan parametreler). Boilerplate'in AES ile şifreleyen `hash.util.ts` dosyası geri çevrilebilir olduğu için kullanılmaz.
- **Access token:** kısa ömürlü JWT (15 dk). Payload: `sub` (userId), `businessId`, `role`. Kişisel veri içermez.
- **Refresh token:** rastgele değer, veritabanında hash'li saklanır, her kullanımda **rotate** edilir. Çıkışta ve şifre değişiminde kullanıcının refresh token'ları silinir. Token iptali için Redis blacklist kullanılmaz; access token'ın kısa ömrü yeterlidir.
- Mobilde token yalnızca expo-secure-store'da durur (RULES.md).
- Giriş, şifre sıfırlama ve public web uçlarında `@nestjs/throttler` ile hız sınırı uygulanır.
### 7.2. Roller

| Rol | Kim | `businessId` |
| --- | --- | --- |
| `owner` | Salon sahibi | Dolu |
| `admin` | Petzibu ekibi | Boş |

- Boilerplate'teki RBAC permissions modülü kullanılmaz; `@Roles('admin')` guard'ı yeterlidir. MVP'de işletme başına tek kullanıcı vardır (K5).
- Personel modülü (Faz 2) geldiğinde `staff` rolü ve yetki modeli o günün ihtiyacına göre tasarlanır. Tenant işletme olduğu için veri modeli hazırdır.
### 7.3. İşletme durumu

`business.status`: `pilot | active | payment_due | suspended | deleting` (K11, K34, K57). Durum alanlarının yanında `graceEndsAt`, `suspendedAt`, `deleteRequestedAt`, `deleteAt` ve `previousStatus` tutulur.

- **Yazma kontrolü tek yerdedir:** global `TenantStatusGuard`, `suspended` veya `deleting` durumundaki işletmenin yazma isteklerini (`POST`, `PUT`, `PATCH`, `DELETE`) `403 TENANT_SUSPENDED` ile reddeder. Okuma çalışır.
- Global guard sırası `app.module.ts`'te tanımlıdır ve bilinçlidir: `JwtAuthGuard` → `RolesGuard` → `TenantStatusGuard` → `ConsentGuard`. İlki `request.user`'ı ve tenant bağlamını kurar, diğerleri onu okur.
- Askıdayken de çalışması gereken yazma uçları (müşteri silme HES-04, hesap silme ve vazgeçme HES-07) `@AllowWhenSuspended()` ile işaretlenir. İstisnalar yalnızca bu dekoratörle tanımlanır.
- Guard durumu her istekte veritabanından okur. MVP'de önbellek yok.
- `GET /me` işletmenin durumunu ve `pendingConsents[]` listesini döner (K58). Onay bekleyen owner'ın diğer istekleri `403 CONSENT_REQUIRED` alır.
### 7.4. İç araç (admin)

- `admin` modülü, `admin` rolüyle korunan API uçlarıdır: işletme açma ve davet (ADM-01), durum değiştirme (ADM-02), metin sürümü yayınlama (ADM-03).
- MVP'de ayrı bir panel yoktur; bu uçlar Swagger veya küçük bir script ile çağrılır. İlk admin kullanıcısı seed script'iyle oluşturulur.
- Admin uçları `runAsSystem()` içinde çalışır (§5.2).
---

## 8. Arka plan işleri

**BullMQ + Redis.** İşler API ile aynı süreçte çalışır; kuyruk tanımları `common/queue` altındadır.

| İş | Tetikleyici | Karar |
| --- | --- | --- |
| `graceEndsAt` geçen işletmeleri Askıda'ya al | Günlük | K34 |
| `suspendedAt` + 11 ay olanlara silme uyarısı e-postası | Günlük | K34 |
| `suspendedAt` + 12 ay ve `deletionRequestedAt` + 30 gün olan işletmeleri sil | Günlük | K34, K57 |
| 30 günü dolan bekleyen intake formlarını sil | Günlük | INT-04 |
| E-posta gönderimi (davet, sıfırlama, uyarı) | Olay anında kuyruğa | |

- Günlük işler BullMQ'nun tekrarlayan job'larıyla (job scheduler) kurulur; saat `Europe/Istanbul`'a göre tanımlanır.
- Her iş `runAsSystem()` ile başlar, işletme başına adımlarda `runAsTenant()` kullanır.
- İşler idempotent yazılır: aynı iş iki kez çalışırsa sonuç değişmez.
- **Hatırlatma kuyruğu bir job değildir.** MVP'de (K3) "gönderilmesi gereken ve henüz gönderilmemiş mesajlar" bir sorgudur ve `reminders` modülü bunu istek anında hesaplar. Otomatik WhatsApp (Faz 2) geldiğinde gönderim BullMQ işine dönüşür.
---

## 9. Dosyalar, PDF ve Excel

### 9.1. Fotoğraflar

- Mobil, API'den **presigned PUT URL** alır ve dosyayı doğrudan S3'e yükler. API yalnızca nesne anahtarını kaydeder.
- Anahtar biçimi: `businesses/{businessId}/{alan}/{kayıtId}/{uuid}.jpg` (ör. `pets/…`, `grooming-reports/…`). İşletme silmede tek önek silinir.
- Bucket private'tır. Okuma kısa ömürlü **presigned GET URL** ile yapılır.
- Boyut ve içerik türü presigned URL'de sınırlanır. Sıkıştırma mobilde yapılır; sunucuda görüntü işleme yoktur.
- Önce/sonra kolajı saklanmaz, telefonda üretilir (K33).
### 9.2. PDF ve Excel

- `document-generator` modülünün adapter'ları kullanılır.
- Döküm PDF'i (K42, `GET /statements/:id/pdf`) ve dışa aktarma Excel'i (K55, `GET /export/xlsx`) istek anında, senkron üretilir ve stream edilir. Dosya saklanmaz.
- Veri büyüdüğünde dışa aktarma bir BullMQ işine taşınır; MVP'de gerek yok.
---

## 10. Public web yüzeyi (K14)

- NestJS içinde, `public-web` modülünde, Handlebars şablonlarıyla sunucu tarafında render edilir. Ayrı web uygulaması yoktur.
- Sayfalar: davet kabulü (KUR-01), şifre sıfırlama (KUR-03), intake formu (E3), tek tıkla onay (K52).
- İstemci tarafı framework yoktur. Form gönderimi düz HTML form + sunucu tarafı doğrulamadır (aynı zod şeması). Stil tek bir CSS dosyasıdır; renkler `theme.ts` token'larından alınır.
- Hesapsızdır; işletme linkteki token'dan bulunur. Askıdaki işletmenin intake ve onay linkleri "Bu işletme şu an form kabul etmiyor" sayfasını gösterir (K34).
- Hız sınırı uygulanır (§7.1).
---

## 11. Ortamlar, Docker ve CI

### 11.1. Yerel geliştirme

`docker-compose.yml` yalnızca bağımlılıkları kaldırır: PostgreSQL, Redis, MinIO ve Mailpit (e-postaları yerelde görmek için). API `pnpm dev` ile host'ta çalışır.

### 11.2. Konfigürasyon

- Env değişkenleri uygulama açılışında **zod** şemasıyla (`config/env.schema.ts`) doğrulanır; zorunlu bir değer eksikse uygulama başlamaz. Joi kullanılmaz.
- Dosyalar: `.env.development`, `.env.test`; üretimde değişkenler ortamdan gelir. `.env.example` her zaman günceldir.
- Üretimde gereken ama yerelde Mailpit ile karşılanan değişkenler (`SENDGRID_API_KEY`, `MAIL_FROM`) opsiyoneldir.
### 11.3. İmaj

- Tek imaj: `apps/api/Dockerfile`, multi-stage. Build context repo köküdür; `turbo prune @repo/api --docker` ile yalnızca API ve bağımlı paketler kopyalanır. Boilerplate'ten kalan npm tabanlı Dockerfile ve CD workflow bu tarife göre yeniden yazılır.
- PDF üretimi Puppeteer kullandığı için imaja Chromium ve bağımlılıkları eklenir.
- Konteyner açılışında `prisma migrate deploy` çalışır, ardından uygulama başlar.
- İmaj adı `petzibu-api:<git-sha>`; sürüm etiketi `vX.Y.Z`.
### 11.4. CI (GitHub Actions)

- Her PR'da: `lint`, `type-check`, `test`, `build`. Turborepo yalnızca etkilenen paketleri çalıştırır.
- API e2e testleri Postgres ve Redis servis konteynerleriyle çalışır; cross-tenant testleri (§5.2) bu aşamadadır.
- Deploy adımı hosting kararına bağlıdır (§14).
---

## 12. Test stratejisi

| Tür | Kapsam | Araç |
| --- | --- | --- |
| Unit | Saf kurallar: fiyat (K32), borç dağıtımı (K41), rebook süresi (K49), uyarı hesabı (K31), yuvarlama | Jest |
| E2E | Her endpoint'in mutlu yolu, domain hataları, cross-tenant erişim, askı guard'ı | Jest + Supertest, gerçek Postgres |

- Veritabanı mock'lanmaz; e2e testleri gerçek Postgres'e karşı çalışır ve her test dosyası temiz bir şemayla başlar.
- Mobil testler RULES.md'deki gibi kalır.
---

## 13. Git stratejisi

- `main` her zaman deploy edilebilir durumdadır.
- İşler kısa ömürlü branch'lerde yapılır (`feat/KUR-01-davet-kabulu`, `fix/...`) ve PR ile **squash merge** edilir.
- Commit mesajları Conventional Commits biçimindedir.
- Sürümler `main` üzerinde `vX.Y.Z` tag'iyle işaretlenir.
- `contracts`'taki bir değişiklik, onu kullanan API ve mobil kodla **aynı PR'da** yapılır. Eski sürümü yaşatmak için versiyonlama yapılmaz.
---

## 14. Açık kararlar

| Konu | Durum |
| --- | --- |
| Hosting ve bölge | Belirlenmedi. KVKK açısından verinin yurt içinde tutulması gerekip gerekmediği hosting seçiminden önce netleşmeli. Mimari Docker ile her ortamda çalışır. |
| Monetization | Plan ve paket yapısı netleşince `subscriptions` modülü eklenir. Bugün ödeme manuel; durum `businesses`'ta. |

Kapatılan kararlar ADR olarak `decisions/` altında yaşar.

---

## 15. Genişleme yolu

| Gelecek | Nereye oturur |
| --- | --- |
| **Pet oteli (lodging)** | Yeni `lodging` modülü (domain katmanı). `customers`, `pets` ve `billing`'i kullanır, `appointments`'a dokunmaz. Oda/kafes kapasitesi, giriş-çıkış tarihleri kendi tablolarında. Fiyat kuralı ortaklaşırsa `appointments`'tan ayrı bir modüle çıkarılır. Dikeyler çoğalınca `modules/` altında `core/`, `grooming/`, `lodging/` gruplaması değerlendirilir. |
| **Personel (Faz 2)** | `users`'a `staff` rolü; yetki modeli o gün tasarlanır. Randevu satırına personel ataması `appointments`'ta. |
| **Otomatik WhatsApp ve push (Faz 2)** | `reminders` gönderimi BullMQ işine taşır; seçilen kanal için yeni bir gönderim modülü yazılır (§4.4). Şablonlar aynı kalır (K3). |
| **Online randevu (Faz 3)** | `public-web`'e yeni sayfa veya web uygulamasına akış; müsaitlik hesabı `appointments` ve `business-hours` üzerinden. |
| **Ücretli abonelik** | `subscriptions` modülü; `businesses.status` geçişleri oradan tetiklenir. |
| **Web back office (K47)** | `apps/web`; `contracts`'ı mobil ile paylaşır. API'de değişiklik gerekmez. |
