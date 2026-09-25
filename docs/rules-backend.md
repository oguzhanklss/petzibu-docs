# Project Rules – Petzibu API

Backend'in kod kuralları. "Neden" sorusunun cevabı [mimari dokümanda](architecture/backend-architecture.md) ve [ADR'lerde](architecture/decisions/index.md); burada yalnızca "nasıl" var. Domain tanımı [mobil RULES.md](rules-mobile.md) ile aynıdır, burada tekrar edilmez.

## Stack

| Alan | Seçim |
| --- | --- |
| Framework | NestJS 11, TypeScript `strict` |
| ORM | Prisma 7, PostgreSQL 16 |
| Doğrulama ve sözleşme | Zod 4 + nestjs-zod, `@repo/contracts` |
| Env | Zod şeması, `config/env.schema.ts` |
| Request context | nestjs-cls, `@nestjs-cls/transactional` |
| Kuyruk | BullMQ + Redis |
| Dosya | S3 uyumlu (yerelde MinIO), presigned URL |
| E-posta | SendGrid (yerelde Mailpit) |
| PDF / Excel | Puppeteer + EJS, ExcelJS |
| Public web | Handlebars, sunucu tarafı render |
| Şifre | argon2id |
| Log | Winston, `LoggerService` |
| Test | Jest, Supertest, gerçek Postgres |

Başka kütüphane için açık bir sebep gerekir. Alan başına tek kütüphane.

## Klasör yapısı

```
src/
  common/        # guards, interceptors, filters, decorators, logger, queue, utils
  config/        # env.schema.ts + registerAs fabrikaları
  database/      # PrismaService, tenant.extension.ts, tenant-context.ts, enum-parity.ts
  health/
  modules/
    <modül>/
      <modül>.module.ts
      <modül>.controller.ts
      <modül>.service.ts
      <modül>.service.spec.ts
```

- Modül sınıfı: platform (mail, files, document-generator), çekirdek (users, auth, businesses, business-hours, legal), domain (catalog, customers, pets, appointments, grooming-reports, billing, expenses), orkestrasyon (reminders, intake, reports, privacy, admin, public-web). Bağımlılık yönü platform ← çekirdek ← domain ← orkestrasyon.
- Modül gerçekten büyüyünce klasörlere ayrılır, önceden değil. `dto/`, `repositories/`, `commands/`, `queries/` klasörleri açılmaz.
- `utils/` çöplüğü yok. Bir yardımcı ikinci kullanıcısı çıkana kadar tek kullanıcısının yanında durur.

## Modül anayasası

1. Bir modül **kendi tablolarının tek sahibidir**. Başka modülün tablosunu Prisma ile okumaz ve yazmaz; veriyi sahibinin servisinden ister.
2. Bağımlılık **tek yönlüdür**. Döngü ve `forwardRef` yasak. İhtiyaç duyuluyorsa mantık yanlış modüldedir; orkestrasyon katmanına taşınır.
3. Yalnızca **doğrudan servis çağrısı**. Event bus, event emitter, CQRS yok. Sınır aşan yan etki çağıran serviste açıkça yazılır.
4. Bir modül yalnızca `exports`'taki servisleri dışarı açar. Controller kendi modülünün servisini çağırır.
5. Modüller arası veri `@repo/contracts` tipleriyle ya da servisin açık dönüş tipiyle taşınır. `any` yasak.

## Veri

- Tenant işletmedir. İşletmeye ait her tabloda `businessId` vardır ve tablo `tenant.extension.ts`'teki `TENANT_MODELS` listesindedir. Sorgularda `businessId` elle yazılmaz; extension ekler. Context yoksa extension `MissingTenantContextError` fırlatır.
- **Prisma'ya `PrismaService.client` üzerinden erişilir**, `this.prisma.customer` değil `this.prisma.client.customer`. `client` bağlama duyarlıdır: `@Transactional()` içindeysen o transaction'ın client'ını döner.
- **`create`'te `businessId` yazmak için `scoped()` kullan.** Extension alanı çalışma zamanında doldurur ama Prisma'nın tipleri zorunlu görür: `data: scoped<Prisma.ExpenseUncheckedCreateInput>({ ... })`.
- Tenant'sız kod `runAsSystem()`, işletme adına çalışan job `runAsTenant(businessId)` içinde çalışır. Başka yol yok. `runAsSystem()` bütün işletmeleri görür: kapsamı küçük tut ve içinde `businessId`'yi elle yaz.
- **Tenant kapsamlı olmayan tablolar** ve nedenleri `tenant.extension.ts` başında yazılıdır: `Business` (tenant'ın kendisi), `User` (admin'in işletmesi yok), `RefreshToken` / `AuthToken` (kimlik doğrulamadan önce okunur), `LegalText` (Petzibu genelinde). Bunlara dokunan sorgular `runAsSystem()` içinde ve kapsamı elle yazar.
- **Yeni tenant tablosu üç şey ister:** `businessId` alanı + index, `TENANT_MODELS`'e satır, cross-tenant e2e testi. Üçü olmadan PR açılmaz.
- Transaction: `@Transactional()` orkestra eden metotta. Servisler `tx` parametresi taşımaz.
- Raw SQL tenant tablolarında yasak. Zorunluysa `businessId` parametresi alan fonksiyon olarak tek dosyada.
- Birincil anahtar `@default(uuid(7))`. Tablo adı `snake_case` çoğul (`@@map`), alan adı `camelCase`.
- Genel soft-delete yok. `deletedAt`, `createdBy`, `updatedBy` alanları eklenmez. Silme kuralı türe göredir (§5.7).
- Prisma şeması modül başına bir dosya: `prisma/schema/<modül>.prisma`. Şema değişince `prisma:generate`, ardından migration.
- **Prisma enum'u eklersen `enum-parity.ts`'e satır ekle.** Contracts ile Prisma arasındaki fark `type-check`'i kırmalı. Sözleşmeye girmeyen sunucu içi enum'lar (`AuthTokenType` gibi) dosyanın başındaki gerekçeli listeye yazılır.

## Para, zaman, telefon

- Para **tam sayı kuruş**, `Int`. Alan adında birim eki yok (`price`, `total`, `amount`). Tek para birimi TRY, `currency` alanı yok. Yüzde indirim yarım yukarı yuvarlanır ve `billing`'de tek fonksiyondadır.
- Zaman anı `timestamptz`, JSON'da ISO 8601 UTC `Z` ile; offset'li biçim reddedilir. Yerel tarih `date` / `YYYY-MM-DD`, yerel saat `time` / `HH:mm`; ikisi de asla `Date` nesnesine çevrilmez. **Dönüşüm tek yerde:** `contracts/primitives.ts` içindeki `isoDateTime`, `localDate` ve `localTime` Prisma'nın `Date`'ini kabul edip string'e çevirir, yani servis katmanı `toISOString()` yazmaz. Yerel tarih ve saat UTC parçalarından okunur. Süre `durationMinutes`, tam sayı, 5'in katı.
- Telefon E.164, `contracts/primitives.phone` ile doğrulanır.
- İşletmenin `timezone` alanı vardır (varsayılan `Europe/Istanbul`); "yarın 18:00", "çalışma saati dışı" hesapları ona göre yapılır.

## API sözleşmesi

- Şemalar `@repo/contracts`'tan gelir. Controller `createZodDto(schema)` ile doğrular, `@ZodSerializerDto(schema)` ile serialize eder. Elle DTO sınıfı, `plainToInstance`, `@Expose` yok.
- Envelope: başarı `{ success: true, status, data, message? }`; liste `{ success: true, status, data: T[], meta }` (`meta` zorunlu, `count` yok); hata `{ success: false, status, code, message, errors?: [{ field?, message, code? }] }`.
- **Sayfalanan uçlarda `@ZodSerializerDto` kullanılmaz.** O dekoratör handler'ın düz dizi döndürmesini bekler; biz `{ data, meta }` döndürüyoruz. Satırları şemadan tek tek geçir: `data: rows.map((r) => petResponse.parse(r))`. Garanti aynı kalır, alan sızmaz.
- `meta` elle kurulmaz, `buildMeta(total, page, limit)` üretir: `hasNextPage` ve `hasPreviousPage` dahil bütün alanlar dolu olmalı.
- **Global interceptor sırası `main.ts`'te bilinçlidir ve değiştirilmez:** `LoggingInterceptor`, `TransformResponseInterceptor`, `ZodSerializerInterceptor`. Nest yanıt yolunda ters sırada işler, yani serializer ham controller değerini ilk görür, zarf sarma ondan sonra gelir. Sıra bozulursa serializer zarfı şemaya karşı doğrulamaya çalışır ve **her istek 500 döner**.
- Tarih aralığı listeleri (takvim) sayfalanmaz: `dateFrom` + `dateTo`, düz dizi.
- Route tanımları Nest'te kalır; `contracts` route bilmez.
- Sözleşme değişikliği onu kullanan API ve mobil koduyla aynı PR'da yapılır. Sürümleme yok.

## Hatalar

- Domain hatası `DomainException(code, status, errors?)` ile fırlatılır. `code` `contracts/errors.ts`'teki enum'dan gelir; yeni kod önce oraya eklenir.
- `message` Türkçe, insan içindir; istemci ona göre dallanmaz. i18n yok.
- Zod doğrulama hatası `400 VALIDATION_FAILED`, `errors[].field` dolu.
- Başka işletmenin kaydı `404 NOT_FOUND` döner; `403` değil.
- Nest'in `NotFoundException`, `BadRequestException` gibi sınıfları yalnızca `code` taşımayan altyapı hataları için; domain kodunda `DomainException`.

## Kimlik ve yetki

- E-posta + şifre, argon2id (`auth/services/password.service.ts`). Access token JWT 15 dk, payload `sub`, `businessId`, `role`; kişisel veri yok. Refresh token rastgele, DB'de SHA-256 hash'li, her kullanımda rotate.
- Davet ve şifre sıfırlama token'ları da rastgele ve SHA-256 hash'li, **tek kullanımlık** (`usedAt`): `auth/services/auth-token.service.ts`. Argon2 yalnızca şifre için; yüksek entropili token'a gerekmez (ADR-0005).
- Roller: `owner` (businessId dolu), `admin` (boş). `@Roles('admin')` + `RolesGuard`. RBAC permission tablosu yok.
- **Global guard sırası `app.module.ts`'te bilinçlidir ve değiştirilmez:** `JwtAuthGuard` → `RolesGuard` → `TenantStatusGuard` → `ConsentGuard`. Nest global guard'ları kayıt sırasıyla çalıştırır ve ilki `request.user` ile tenant bağlamını kurar.
- **Controller'da `@UseGuards(JwtAuthGuard)` yazılmaz.** Controller guard'ları global guard'lardan sonra çalışır; kimlik doğrulaması orada kurulursa sıra bozulur. Varsayılan korumalı; public uç `@Public()`.
- `TenantStatusGuard` askıdaki işletmenin yazma isteğini `403 TENANT_SUSPENDED` ile keser; istisna yalnızca `@AllowWhenSuspended()`. `ConsentGuard` güncel metni onaylamamış owner'ı `403 CONSENT_REQUIRED` ile keser; istisna yalnızca `@AllowWhenConsentPending()` (onay akışının kendi uçları).
- Hesabın varlığı sızdırılmaz: giriş hatası, şifresiz hesap ve bilinmeyen e-posta **aynı** `401`'i döner; `POST /auth/forgot-password` her durumda `204`.
- Throttler global (`common/throttle.config.ts`); kimlik uçları `@Throttle(AUTH_THROTTLE)` taşır. `ttl` **milisaniyedir**.

## Kod kuralları

- Fail fast: ön koşul sağlanmıyorsa fırlat. "Ne olur ne olmaz" dalı yok.
- Doğrulama yalnızca sınırda (istek gövdesi, env, dış servis yanıtı). Tip sisteminin garanti ettiği şey için runtime kontrol yazılmaz.
- `any` yok; `!` yalnızca sebebini açıklayan yorumla.
- `console.log` yok; `LoggerService`. `requestId` ve `businessId` her satıra CLS'ten **otomatik** girer (`common/logger/log-context.ts`), çağrı yerinde yazılmaz. Kişisel veri girmez; sırrı yolunda taşıyan uçların URL'i `redactUrl`'den geçer.
- Dış servis çağrıları (S3, SendGrid) `RetryService` ile sarılır.
- Cerrahi değişiklik: yalnızca görevin gerektirdiği dosyaya dokun.
- Türkçe: kullanıcıya dönen metinler ve doküman. İngilizce: kod, tanımlayıcılar, commit mesajı.

## Test

- Unit: saf kurallar (fiyat K32, borç dağıtımı K41, rebook K49, uyarı K31, yuvarlama). Prisma mock'lanmaz.
- E2E: her endpoint'in mutlu yolu, domain hataları, **cross-tenant erişim**, askı guard'ı. Gerçek Postgres, her test dosyası temiz şema.
- Yeni tenant tablosu ekleyen PR, o modül için cross-tenant testi de ekler.

## Not now

Mikroservis, in-process event bus, repository katmanı, RBAC permission tablosu, Redis cache, WebSocket, Postgres RLS, client codegen, i18n, push bildirimi, otomatik WhatsApp gönderimi.
