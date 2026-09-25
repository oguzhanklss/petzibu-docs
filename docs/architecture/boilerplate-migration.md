# Petzibu – Boilerplate'ten Petzibu Backend'e Geçiş

> Durum: v0.7 · Faz 0, 1, 2, 3, 4 ve 6 kapandı (25 Eylül 2026). Faz 5'ten yalnızca Node sürümü hizalaması ve log gözden geçirmesi kapandı; kalan maddeler **deploy ve hosting kararı verilene kadar bekliyor** (bilinçli). Sıradaki Faz 7 (docs yayını)
> Amaç: [Backend Mimari Dokümanı](backend-architecture.md) ile eldeki boilerplate arasındaki yapısal farkları tespit etmek ve kod yazımına başlamadan önce kapatılacak işleri sıralı bir liste hâline getirmek.
> Bu belge geçici. Bütün maddeler kapanınca silinir; kalıcı kurallar mimari dokümanda ve `CLAUDE.md`'de yaşar.

Alınan kararlar (bu belgeyi şekillendiren):

- Mobil uygulama monorepo'ya taşınır (`apps/mobile`); zod şemaları `packages/contracts` üzerinden paylaşılır.
- i18n backend'den tamamen çıkar. Mesajlar Türkçe ve inline; istemci `message`'a göre dallanmaz.
- Şifreler **argon2id** ile hash'lenir (mimari doküman §7.1 buna göre güncellenir).
- **Bütün** dokümanların kanonik yeri kod reposudur (`docs/` ve `RULES.md` dosyaları); `petzibu-docs` yalnızca yayın kabuğudur ve içeriği build sırasında buradan çeker. Epic'ler ve ürün kararları K1…K58 de artık `docs/epics/` altında yaşar (ADR-0008).

---

## 1. Yapısal farklar

Sol sütun mimari dokümanın istediği, orta sütun boilerplate'te bugün olan, sağ sütun yapılacak iş.

### 1.1. Doğrulama ve sözleşme

| Mimari doküman | Boilerplate | İş |
| --- | --- | --- |
| Zod 4 + nestjs-zod, tek kaynak `packages/contracts` (§6.1) | class-validator + class-transformer, DTO sınıfları her modülde, `packages/types` boş stub | `packages/contracts` kurulur, DTO sınıfları silinir |
| `ZodValidationPipe` global, `ZodSerializerInterceptor` response'ları şemadan geçirir | `I18nValidationPipe` (whitelist + forbidNonWhitelisted), response serializer yok | Pipe ve interceptor değişir |
| Env doğrulaması zod (§14 açık karar) | Joi (`config/env-validation.schema.ts`) | Joi çıkar, zod şeması gelir |
| Swagger zod şemalarından üretilir | `@nestjs/swagger` dekoratörleri DTO sınıflarında, `common/swagger/api-response.factory.ts`, Postman üretici | nestjs-zod'un swagger entegrasyonu; Postman servisi silinir |
| Enum'lar `contracts`'ta, Prisma enum'larıyla tip seviyesinde parite (`enum-parity.ts`) | Enum'lar Prisma'da ve `common/enums`'ta ayrı ayrı | `enum-parity.ts` yazılır |

### 1.2. Envelope ve hata modeli

| Mimari doküman | Boilerplate | İş |
| --- | --- | --- |
| Liste: `meta` zorunlu, `count` yok (§6.2) | `TransformResponseInterceptor` `count` fallback üretir; `BasePaginatedResponse.count` zorunlu, `meta` opsiyonel | `count` her yerden kaldırılır |
| Hata gövdesinde `code` zorunlu (§6.3) | `SentryExceptionFilter` `{ success, status, message, errors? }` üretir, `code` yok | `DomainException(code, status, errors?)` + filter yeniden yazılır |
| `errors[].field` form alanına eşlenir | class-validator mesaj dizisi olduğu gibi geçiyor | Zod issue → `{ field, message, code }` dönüşümü |
| `message` i18n'siz, Türkçe | Interceptor ve filter `I18nService` ile mesaj çeviriyor | i18n bağımlılığı sökülür |

### 1.3. Tenant, transaction, kimlikler

| Mimari doküman | Boilerplate | İş |
| --- | --- | --- |
| `Business` tenant, her tenant tablosunda `businessId` (§5.2) | `Business` modeli yok, hiçbir tabloda `businessId` yok | Şema sıfırdan |
| nestjs-cls + tenant Prisma extension, context'siz sorgu hata fırlatır | CLS yok; onun yerine `soft-delete.extension.ts` var | Soft-delete extension silinir, tenant extension yazılır |
| `runAsSystem()` / `runAsTenant()` | Yok | `database/tenant-context.ts` |
| `@nestjs-cls/transactional` + Prisma adapter (§5.3) | Yok | Kurulur, `PrismaService` tek extension'lı client verir |
| UUID v7 (`uuid(7)`) (§5.4) | `uuid()` v4 | Şema yazılırken |
| Genel soft-delete yok (§5.7) | `deletedAt` User, File, Announcement'ta; extension otomatik filtreliyor | Alanlar ve extension kalkar |
| `createdBy` / `updatedBy` yok | User'da var | Kalkar |

### 1.4. Kimlik doğrulama ve yetki

| Mimari doküman | Boilerplate | İş |
| --- | --- | --- |
| E-posta + şifre, tek yol (K8) | E-posta + şifre **ve** telefon/OTP, çoklu provider (`UserProvider`, `OTPVerification`, `AuthProvider`) | OTP, provider ve telefon doğrulama akışları silinir |
| argon2id | `hash.util.ts` AES-256-GCM ile **şifreliyor** (geri çevrilebilir); `bcrypt` bağımlılığı hâlâ `package.json`'da | `argon2` gelir, `hash.util.ts` ve `AES_SECRET_KEY` zorunluluğu kalkar |
| `User.email` zorunlu ve unique; telefon yok | `email` opsiyonel, `phoneNumber` zorunlu ve unique | Şema |
| Refresh token rotate, DB'de hash'li | `RefreshToken` modeli var; rotate davranışı doğrulanacak | `token.service.ts` gözden geçirilir |
| İki rol: `owner`, `admin`; `@Roles()` guard (§7.2) | RBAC: `Module`, `Permission`, `Role`, `RolePermission`, `PermissionsGuard`, `permission:sync` script'i | Hepsi silinir, `User.role` enum olur |
| `TenantStatusGuard` + `@AllowWhenSuspended()` (§7.3) | Yok | Yazılır |
| `CONSENT_REQUIRED` kontrolü (K58) | Yok | `legal` modülüyle birlikte |
| Throttler giriş, sıfırlama ve public web'de | Yalnızca `auth.module.ts`'te | Global kurulum, route bazlı limitler |

### 1.5. Modüller

| Mimari doküman | Boilerplate | İş |
| --- | --- | --- |
| Servis + doğrudan Prisma; CQRS ve repository yok (§4.3) | `announcements` CQRS, `users` repository pattern; `CqrsModule.forRoot()` AppModule'de | `announcements` silinir, `users` düzleştirilir, `@nestjs/cqrs` çıkar |
| In-process event bus yasak (§4.2) | `@nestjs/event-emitter` kurulu, kullanılmıyor | Bağımlılık çıkar |
| `sms` ve `notifications` uykuda, AppModule'e import edilmez, env opsiyonel (§4.4) | İkisi de import edilmiş; `FONIVA_*` ve `SENDGRID_*` env'leri zorunlu | **Karar değişti:** ikisi de silindi (Faz 1 sapma 1); `SENDGRID_*` opsiyonel oldu. §4.4 güncellenecek |
| Redis yalnızca BullMQ için; guard önbelleksiz | `cache-manager` + `CacheService`, yalnızca `document-generator` kullanıyor | `cache-manager` çıkar, `document-generator` cache arayüzü kaldırılır |
| BullMQ job scheduler (§8) | `@nestjs/schedule` kurulu | Çıkar |
| WebSocket yok | `@nestjs/websockets`, `socket.io` kurulu, kullanılmıyor | Çıkar |
| `document-generator` adapter'ları döküm PDF ve dışa aktarma Excel için | Örnek adapter'lar: invoice, ticket-detail, tickets-report, sales-report | Örnekler silinir, iskelet kalır |
| `files`: presigned PUT/GET, anahtar `businesses/{id}/...` (§9.1) | Multer ile sunucudan yükleme, `File` modeli, `deletedAt` | Presigned akışa çevrilir |
| `mail`: davet, sıfırlama, askı uyarısı | SendGrid + `welcome`, `verification`, `password-reset` şablonları | Şablonlar Petzibu'ya göre yeniden yazılır; yerelde Mailpit |
| `health` var | Var (terminus) | Kalır |
| `docs` modülü (Swagger yönlendirme) | Var | ~~Gözden geçirilir~~ Silindi; Swagger `main.ts`'te (Faz 1) |
| `AuditLog` modeli | Var | Mimari dokümanda yok; MVP'de kalkar |

### 1.6. Monorepo ve paketler

| Mimari doküman | Boilerplate | İş |
| --- | --- | --- |
| `apps/api`, `apps/mobile`, `packages/contracts` (§3) | `apps/api`, `packages/types` (stub) | `types` → `contracts`; mobil taşınır |
| `contracts` tsup ile derlenir, `^build` bağımlılığı | `turbo.json` `^build` zaten var | tsup config |
| Tek Node sürümü | Mobil `engines: node >=24 <25`; API `@types/node 22`; CD workflow Node 20 | Kökte `.nvmrc`, `engines`, CI node sürümü eşitlenir |

### 1.7. Docker, CI/CD, ortam

| Mimari doküman | Boilerplate | İş |
| --- | --- | --- |
| `docker-compose.yml`: Postgres, Redis, MinIO, Mailpit (§11.1) | Kök compose: yalnızca `postgres:latest`, `redis:latest` | MinIO ve Mailpit eklenir, imajlar pinlenir |
| Dockerfile multi-stage, context repo kökü, `turbo prune` (§11.3) | `apps/api/docker/Dockerfile` npm + `COPY package*.json`; pnpm monorepo'da çalışmaz | Yeniden yazılır |
| CD: tag → imaj (§11.3) | `cd-production.yml` `npm ci`, Node 20, `./docker/Dockerfile` context `.`; monorepo öncesinden kalma, kırık | Yeniden yazılır (deploy hedefi §14'te açık) |
| CI: lint, type-check, test, build; e2e Postgres + Redis servisleriyle (§11.4) | `ci.yml` pnpm tabanlı, Postgres 15 + Redis 7 servisleri var | Büyük ölçüde uygun; `contracts` build adımı eklenir |
| `.env.development`, `.env.test`; `.env.example` güncel (§11.2) | `jest-e2e-setup.ts` `hrsync:hrsync123@localhost:5433/boilerplate_test` hardcode ve `NODE_ENV=development` | `.env.test` ile değiştirilir |
| Konteyner açılışında `prisma migrate deploy` | Dockerfile'da yok | Entrypoint |
| `docker:*` script'leri | `apps/api/package.json`'daki script'lerin yarısı `./docker-compose.yml` (yok), yarısı `../../docker-compose.yml` arıyor | Temizlenir |

### 1.8. Depo hijyeni

| Durum | İş |
| --- | --- |
| `CLAUDE.md` CRM boilerplate için yazılmış: "CRM domain modülleri için CQRS", repository pattern zorunlu, `permission:sync`, `AES_SECRET_KEY` | Baştan yazılır; mimari dokümanın §4.2 anayasasını ve §4.3'ü yansıtır |
| `README.md` "Perfect Boilerplate API" | Petzibu için yazılır |
| `docs/plans/*.md`, `docs/migrations/*.md` CRM'den kalma | Silinir |
| `apps/api/SETUP.md`, `scripts/quick-setup.ts`, `scripts/permission-sync.ts`, `bin/` CLI (inquirer, figlet, chalk) | Silinir |
| `prisma/factories`, `prisma/seeders`, `@faker-js/faker` prod bağımlılığında | Seed yeniden yazılır (ilk admin + örnek işletme); faker devDependency olur |
| `.husky/pre-commit` lint + test + build çalıştırıyor | Lint + type-check yeterli; test ve build CI'da |
| Swagger başlığı "Boilerplate API", `BULLMQ_PREFIX` varsayılanı `perfect-boilerplate` | Düzeltilir |
| `templates/pdf` örnekleri | Silinir |

---

## 2. TODO

Sıra bağımlılığı izler: önce kurallar ve temizlik, sonra sözleşme katmanı, sonra veri katmanı, sonra auth, sonra altyapı, en son mobil taşıma. Her faz sonunda `pnpm lint && pnpm type-check && pnpm test && pnpm build` yeşil olmalı.

### Faz 0 · Kararlar ve dokümanlar — kapandı

- [x] Mimari doküman güncellemeleri:
  - [x] §2 tabloya "i18n: yok" ve "Şifre hash: argon2id" satırları
  - [x] §7.1 bcrypt → argon2id
  - [x] §14 "Config doğrulaması" kararını kapat: zod *(karar açık kararlar tablosundan çıktı; §11.2 zod diyor)*
  - [x] §5.7'ye not: boilerplate'in soft-delete extension'ı kaldırıldı
  - [x] §11.3 ile mevcut CD workflow çelişkisi not düşülür (workflow yeniden yazılacak)
  - [x] §4.4'ten `sms` ve `notifications` "uyuyan modül" maddesi çıkar (Faz 1'de silindiler) *(§4.4 "Bildirim kanalları" oldu; §4.1 haritası, §11.2 ve §15 çapraz referansları da düzeltildi)*
  - [x] Başlıktaki `[RULES.md](/docs/rules.md)` linki göreli yol olur
- [x] `CLAUDE.md` baştan yazılır (modül anayasası, servis + Prisma, contracts, tenant kuralları, komutlar)
- [x] `README.md` Petzibu için yazılır
- [x] `docs/plans/` ve `docs/migrations/` silinir
- [x] `docs/architecture/decisions/` altında kısa ADR'ler: contracts + zod, argon2id, i18n yok, CQRS ve repository yok, tenant extension, UUID v7, mobil monorepo'da

### Faz 1 · Temizlik (kaldır) — kapandı

Kapanış tarihi: 25 Eylül 2026. `pnpm --filter @repo/api lint | type-check | test | build` dördü de yeşil; AppModule yalnızca Config, Logger, Common, Queue, Prisma, Auth, Users, Files, Mail, DocumentGenerator, Health import ediyor.

- [x] `modules/announcements`, `@nestjs/cqrs`, `CqrsModule.forRoot()`
- [x] `modules/permissions`, `PermissionsGuard`, `@Permissions()`, `permission:sync`, `Module` / `Permission` / `Role` / `RolePermission` modelleri
- [x] `modules/users/repositories`, `USER_REPOSITORY`; servis doğrudan Prisma kullanır
- [x] `modules/i18n`, `nestjs-i18n`, `I18nModule`, `I18nValidationPipe`, `I18nValidationExceptionFilter`; interceptor ve filter'daki `I18nService` bağımlılığı
- [x] `@nestjs/event-emitter`, `@nestjs/websockets`, `@nestjs/platform-socket.io`, `socket.io`, `@nestjs/schedule`, `@nestjs/mapped-types`
- [x] `cache-manager`, `cache-manager-redis-store`, `@nestjs/cache-manager`, `common/services/cache.service.ts`; `document-generator`'daki cache arayüzü
- [x] `bcrypt`, `@types/bcrypt`; `common/utils/hash.util.ts` (Faz 4'te argon2 ile yeniden yazılır)
- [x] `database/soft-delete.extension.ts`
- [x] Auth'tan OTP ve provider akışları: `otp.service.ts`, `user-provider.service.ts`, OTP DTO'ları, `OTPVerification` ve `UserProvider` modelleri, `AuthProvider` enum
- [x] `AuditLog` modeli ve `log.prisma`
- [x] `common/postman/`, `openapi-to-postmanv2`
- [x] `bin/`, `scripts/quick-setup.ts`, `scripts/setup.ts`, `scripts/permission-sync.ts`, `scripts/add-service-comment.mjs`, `SETUP.md`; `inquirer`, `figlet`, `chalk`
- [x] `templates/pdf/*` ve `document-generator/adapters/*` örnekleri (base sınıflar ve factory kalır)
- [x] `mail/templates/*` örnekleri (Faz 4'te davet ve sıfırlama şablonları yazılır)
- [x] `common/utils` içinde Petzibu'ya ait olmayanlar: `turkish-id.validator.ts`, `otp-message.util.ts`, `phone-validator.util.ts`; `phone-normalizer.util.ts` E.164 için `contracts/primitives`'e taşınır *(taşıma Faz 2'de; dosya şimdilik `common/utils`'te)*
- [x] `common/enums/domain-status.enum.ts`, `action.enum.ts` *(klasör tamamen kalktı)*
- [x] `@faker-js/faker` devDependency'ye; `prisma/factories` ve `prisma/seeders` silinir
- [x] `apps/api/package.json` `docker:*` script'leri temizlenir
- [x] ~~`SmsModule` ve `NotificationsModule` `AppModule`'den çıkar~~ → **karar değişti:** ikisi de tamamen silindi (bkz. aşağıdaki sapmalar)

#### Faz 1'de plandan sapmalar

1. **`sms` ve `notifications` silindi, uykuya alınmadı.** Mimari doküman §4.4 bunları "uykuda" tutuyordu; ancak 41 dosyanın tamamı i18n ve RBAC'a bağlıydı ve Faz 2 DTO'larını zaten zod'a çevirecekti. Petzibu'nun SMS ve push ihtiyacı henüz hiçbir epic'te tanımlı değil. Silindi; ihtiyaç doğduğunda `90b8e2e` commit'inden geri alınabilir. `notifications.prisma`, `foniva.config.ts`, `firebase-admin` ve `FONIVA_*` env'leri de birlikte kalktı. **Mimari doküman §4.4 buna göre güncellenmeli.**
2. **Prisma şeması yalnızca derlenecek kadar dokunuldu, migration üretilmedi.** `User`'dan `role` / `auditLogs` / `otpVerifications` / `userProviders` ilişkileri, `auth.prisma`'dan `AuthProvider` / `UserProvider` / `OTPVerification`, `file.prisma`'dan `announcements` ilişkisi silindi. `deletedAt` / `createdBy` / `updatedBy` alanları **duruyor**: Faz 3 şemayı sıfırdan yazacak ve o zaman kalkacak. Eski migration (`20260506105839_initial_migration`) da Faz 3'te silinir. Bu yüzden şu an şema ile dev veritabanı arasında drift var; `prisma migrate reset` Faz 3'te yapılacak.
3. **Auth iskelete indi.** `register`, `login`, `forgot-password`, `reset-password` uç noktaları kalktı: `User`'da `passwordHash` yok (şifre `UserProvider`'da tutuluyordu) ve argon2id Faz 4'te geliyor. Ayakta kalanlar: `POST /auth/refresh` (rotate), `POST /auth/logout`, `GET /auth/me`. `TokenService` ve `JwtStrategy` olduğu gibi korundu.
4. **`users` modülünden rol uç noktaları kalktı:** `POST /users/:id/roles`, `DELETE /users/:id/roles/:roleId`, `GET /users/:id/roles`, `GET /users/me/roles`. `@Roles()` + `RolesGuard` Faz 4'te gelecek.
5. **`files` listeleme geçici olarak yalnızca sahibin dosyalarını döndürüyor.** `AuthorizationService.hasPermission('FILES.VIEW_ALL')` yerine sabit `false`. Faz 4'te `@Roles('owner')` ile işletme sahibine tümünü görme hakkı verilecek.
6. **`DocsModule` silindi** (`common/postman` bağımlısıydı). Swagger kurulumu `main.ts`'te kaldı; migration belgesinin §1.5'teki "muhtemelen `main.ts`'e katlanır" öngörüsü gerçekleşti.
7. **class-validator doğrulama mesajları şimdilik İngilizce (kütüphane varsayılanı).** `i18nValidationMessage(...)` sarmalayıcıları söküldü ama Türkçe mesaj yazılmadı: bu DTO'lar Faz 2'de zod şemalarıyla tamamen değişecek. Servis ve filtre katmanındaki hata mesajları Türkçe yazıldı.
8. **Silinen davranışın testleri silindi, kalanlar onarıldı.** Kaldırılanlar: cache entegrasyon testi, `document-generator` cache senaryoları, `template-engine` i18n enjeksiyon testleri, `transform-response` i18n bloğu. Onarılanlar: `s3`, `mail`, `sendgrid`, `jwt.strategy`, `files.controller` ve iki e2e spec (Türkçe mesaj beklentileri).
9. **Faz 5'ten öne alınanlar:** `.husky/pre-commit` lint + type-check'e indi, `BULLMQ_PREFIX` varsayılanı `petzibu` oldu, `.env.example` Petzibu'ya göre yeniden yazıldı (`AES_SECRET_KEY`, `CHAT_ENCRYPTION_KEY`, `FIREBASE_*`, `REDIS_TTL`, `MONGO_*` kalktı). `docker-compose.yml` (MinIO + Mailpit, pinli imajlar) ve Dockerfile hâlâ Faz 5'te.
10. **Ek silinenler (plandaki listede yoktu):** `common/utils/crypto.util.ts` (AES-256-GCM, `AES_SECRET_KEY` bağımlısı), `scripts/check-extensions.ts`, `scripts/README.md`, `document-generator/enums/cache-strategy.enum.ts`, `axios` ve `@nestjs/axios`.

Bağımlılık temizliğinin net etkisi: `pnpm install` sonrası 187 paket düştü.

### Faz 2 · Sözleşme katmanı — kapandı

Kapanış tarihi: 25 Eylül 2026. `pnpm lint | type-check | test | build` dördü de yeşil (API 35 suite / 497 test, mobil 8 suite / 55 test).

- [x] `packages/types` → `packages/contracts`; `tsup` ile ESM + CJS + d.ts; `turbo.json` `^build` zaten var
- [x] `contracts/src/primitives.ts`: `id`, `isoDateTime` (yalnızca `Z`), `localDate`, `localTime`, `money`, `durationMinutes` (`multipleOf(5)`), `phone` (E.164). Kaynak: mobildeki `src/lib/schemas.ts`
- [x] `contracts/src/envelope.ts`: `envelope()`, `paginated()`, `errorBody`, `paginationMeta`, `listQuery`
- [x] `contracts/src/errors.ts`: `ErrorCode` enum (§6.3 tablosu başlangıç)
- [x] `contracts/src/enums.ts`: `AppointmentStatus`, `BusinessStatus`, `Species`, `SizeTier`, `UserRole`, `PaymentMethod`, `ExpenseCategory`, `AlertTag`
- [x] `nestjs-zod` kurulur; Zod 4 ve `@nestjs/swagger` sürüm uyumu doğrulanır
- [x] `ZodValidationPipe` global
- [x] `ZodSerializerInterceptor` global; controller'lar `@ZodSerializerDto(schema)` ile işaretlenir
- [x] `TransformResponseInterceptor` sadeleştirilir: `count` kalkar, `meta` zorunlu, `message` opsiyonel Türkçe
- [x] `DomainException(code, status, errors?)` ve `common/filters/domain-exception.filter.ts`; `SentryExceptionFilter` yalnızca bilinmeyen hataları yakalar
- [x] Zod hataları `400 VALIDATION_FAILED`, `errors[].field` doldurulur
- [x] `common/base/dto/*` silinir
- [x] Env doğrulaması zod'a taşınır: `config/env.schema.ts`; Joi çıkar
- [x] Swagger nestjs-zod ile şemalardan üretilir; başlık "Petzibu API"
- [x] `database/enum-parity.ts`: contracts enum'ları ile Prisma enum'larının tip seviyesinde eşitliği *(yardımcı yazıldı; kontroller Faz 3'te şemaya enum girdiğinde açılacak — dosyada yorumlu duruyor)*

#### Faz 2'de öğrenilenler ve plandan sapmalar

1. **Global interceptor sırası kritik.** Nest yanıt yolunda ters sırada işler. `ZodSerializerInterceptor` ham controller değerini görmeli, zarf sarma ondan sonra gelmeli. `main.ts`'teki sıra bu yüzden `LoggingInterceptor`, `TransformResponseInterceptor`, `ZodSerializerInterceptor`. Yanlış sırada her istek 500 döndü; `RULES.md`'ye kural olarak yazıldı.
2. **`@ZodSerializerDto([Dto])` sayfalanan uçlarda çalışmaz.** Dizi biçimi handler'ın **düz dizi** döndürmesini bekler; bizim liste uçları `{ data, meta }` döndürüyor. Çözüm: satırları şemadan tek tek geçirmek (`rows.map((r) => schema.parse(r))`). Alan sızdırmama garantisi korunuyor. Kural `RULES.md`'de.
3. **`contracts` iki tür yardımcı ihraç eder.** Backend Swagger için ham şemaya ihtiyaç duyar (`envelope`, `paginated`), istemci ise doğrudan `data` almak ister. Bu yüzden `unwrap()` ve `unwrapPaginated()` ayrı fonksiyonlar olarak eklendi; mobil Faz 6'da bunları kullanacak.
4. ~~**Mobil henüz `contracts`'ı import etmiyor.**~~ Faz 6'da kapandı: `src/lib/schemas.ts` silindi, `features/*/schema.ts` yalnızca form şemalarına indi.
5. **`Species` iki ayrı enum oldu.** Hayvanın türü `species` (`dog` | `cat`, K19), hizmetin uygulandığı tür `serviceSpecies` (`dog` | `cat` | `both`, K36). Mobil bunları zaten ayrı tutuyordu; plan tek isim yazıyordu.
6. **`PaymentMethod` ve `ExpenseCategory` epic'lerden türetildi.** Ödeme yöntemleri E7'den (`cash`, `card`, `transfer`), gider kategorileri K44'ten (`supplies`, `rent`, `utilities`, `equipment`, `other`).
7. **`users` modülü örnek uç nokta olarak dönüştürüldü.** `contracts/src/users.ts` bugünkü `User` modelini yansıtır (telefon zorunlu, e-posta opsiyonel, rol yok). Faz 3/4'te hedefe çekilecek: `email` zorunlu, `role` enum, `businessId`.
8. **Faz 1'den kalan iki kalıntı kapandı:** `common/utils/crypto.util.ts` zaten silinmişti; `common/swagger/api-response.factory.ts` `count` yerine `meta` üretiyor ve hata gövdesine `code` eklendi. `files.service.listFiles` eksik `meta` döndürüyordu (`hasNextPage` / `hasPreviousPage` yoktu), `buildMeta`'ya çevrildi.
9. **Ek kurulum işleri (planda yoktu):** `packages/tsconfig/base.json`'dan geçersiz `ignoreDeprecations: "6.0"` kaldırıldı (TS 5.9 kabul etmiyor, tsup dts derlemesini kırıyordu); `pnpm-workspace.yaml`'a `esbuild: true` eklendi (tsup için); `contracts` paketine kendi flat ESLint kurulumu verildi.
10. **Kontrol noktası testi eklendi:** `src/common/__tests__/contracts-pipeline.spec.ts` doğrulama, serileştirme, zarf ve hata modelini veritabanı olmadan uçtan uca sınar (11 test). Faz 2'nin kanıtı bu dosyadır.

### Faz 3 · Veri katmanı — kapandı

Kapanış tarihi: 25 Eylül 2026. `pnpm lint | type-check | test | build` yeşil; cross-tenant e2e testi 17 iddiayla geçiyor; uygulama `docker compose up -d` sonrası ayağa kalkıyor ve zarf ile hata gövdesi gerçek istekte doğrulandı.

- [x] `prisma/schema/*.prisma` sıfırdan, modül başına dosya
- [x] Bütün PK'lar `@default(uuid(7))`
- [x] Tenant tablolarında `businessId` + index; `deletedAt`, `createdBy`, `updatedBy` yok
- [x] `Business.status` enum + `graceEndsAt`, `suspendedAt`, `timezone`, `onboardingCompletedAt`
- [x] `nestjs-cls` kurulur; `ClsModule.forRoot({ middleware: { mount: true } })`
- [x] `database/tenant.extension.ts`: tenant tablo listesi tek yerde; `where`'e `businessId` enjekte, `create`'te doldur, context yoksa fırlat
- [x] `database/tenant-context.ts`: `runAsSystem()`, `runAsTenant(businessId)`
- [x] `@nestjs-cls/transactional` + `@nestjs-cls/transactional-adapter-prisma`; adapter'a tenant extension'lı client verilir
- [x] `PrismaService` tek client döner; `prisma-exception.mapper.ts` `DomainException`'a uyarlanır
- [x] `prisma/seed.ts`: ilk admin + bir pilot işletme + owner
- [x] `database/enum-parity.ts` içindeki kontroller açıldı
- [x] İlk migration oluşturuldu (`20260925081745_petzibu_initial_schema`); `scripts/migrate.ts` gerekmedi, `prisma migrate dev` yeterli
- [x] Cross-tenant e2e test yardımcıları: iki işletme, iki token, `404` beklentisi (`test/utils/tenant-fixtures.ts`)
- [ ] `@db.Date` / `@db.Time` alanları serializer'da string'e çevrilir (§5.6) → **Faz 4'e taşındı**, aşağıdaki 8. maddeye bakın

#### Şema: modül başına dosya

`businesses` (+ çalışma saatleri, kapalı günler), `users`, `auth`, `legal`, `catalog`, `customers`, `pets`, `appointments`, `grooming-reports`, `billing`, `expenses`, `reminders`, `intake`, `files`. Toplam 28 model, 20 enum.

#### Faz 3'te öğrenilenler ve plandan sapmalar

1. **`PrismaService` artık client'ın kendisi değil, ona bağlama duyarlı bir kapı.** Servisler `this.prisma.client.customer` yazar. Sebep: `@Transactional()` içindeyken aynı çağrı o transaction'ın client'ını dönmeli. `TransactionHost.tx` bunu sağlıyor, ama transaction adapter enjeksiyon token'ında **client** bekliyor. Bu yüzden client `PrismaClientHost` + `PRISMA_CLIENT` token'ıyla ayrı sağlanıyor. Kural `RULES.md`'de.
2. **`scoped()` yardımcısı eklendi.** Extension `create`'te `businessId`'yi çalışma zamanında doldurur ama Prisma'nın üretilmiş tipleri onu zorunlu görür. `scoped<Prisma.XCreateInput>({...})` bu boşluğu kapatıyor, böylece `businessId` sorgulara elle yazılmıyor.
3. **`enum-parity.ts`'in ilk hâli sessizce hiçbir şey yakalamıyordu.** `Exact<A,B> extends true ? [] : [never]` yazımında uyuşmazlık `never` üretiyor ve `never extends true` TypeScript'te **doğru**. Koşul demet içine alındı (`[IsExact<A,B>] extends [true]`) ve `IsExact` `never` yerine `false` dönüyor. Kasıtlı bir uyuşmazlıkla probe edilerek doğrulandı.
4. **Mekanizma düzeltilince gerçek bir hata buldu:** contracts'taki `businessStatus` enum'unda `deleting` eksikti (K57). Eklendi. Mobildeki kopyası Faz 6'da tamamen kalktı.
5. **`Business.deleteRequestedAt` + `deleteAt` kullanıldı**, plandaki `deletionRequestedAt` değil: E8 (HES-07) bu iki alanı adıyla tanımlıyor. `previousStatus` da eklendi (silme talebinden vazgeçme).
6. **`Species` iki enum:** hayvanın türü `Species` (`dog` | `cat`, K19), hizmetin uygulandığı tür `ServiceSpecies` (`dog` | `cat` | `both`, K36).
7. **`AppointmentLine.appointmentPetId` nullable.** Hizmet satırı hep bir hayvana bağlıdır ama ek ücret satırı salon düzeyinde olabilir (küçük perakende, KUR-08). Satır bu yüzden `appointmentId`'yi de taşır.
8. **`@db.Date` / `@db.Time` dönüşümü Faz 4'e kaldı.** Prisma bu alanları `Date` olarak döndürüyor; `contracts`'taki `localDate` ve `localTime` string bekliyor. Dönüşüm tek yerde, serializer'da yapılacak. Bugün bu alanları okuyan bir uç nokta yok, o yüzden kırık bir davranış yok; ilk tarih taşıyan uç noktayla birlikte yazılacak.
9. **`User` ve `File` modelleri Petzibu'ya göre yeniden yazıldı, bu da iki modülü sürükledi.** `User`: e-posta zorunlu ve unique, `passwordHash` (Faz 4'te dolacak), `role` enum, `businessId` nullable; telefon, profil fotoğrafı ve `deletedAt` kalktı. `File`: `businessId` + `key` (önek `businesses/{id}/`), sahiplik ve soft-delete kalktı. Sonuç: `files` modülünden bütün izin parametreleri ve sahiplik kontrolleri silindi (erişim denetimi artık tenant izolasyonundan geliyor), `users` modülü rol doğrulamasıyla yeniden yazıldı, `token.service` ve `jwt.strategy` yeni payload'a (`sub`, `businessId`, `role`) geçti. `GET /auth/me` e-postayı veritabanından okuyor: token kişisel veri taşımaz (§7.1).
10. **`contracts`'a `users.ts` hizalandı ve `files.ts` eklendi.** Faz 2'de yazılan `users.ts` eski modeli yansıtıyordu.
11. **`SendGridProvider` açılışta hata fırlatıyordu ve uygulama yerelde hiç ayağa kalkmıyordu.** Anahtar yoksa artık uyarı logluyor ve `isConfigured` false dönüyor; hata gönderim anında veriliyor. Bu Faz 1'de env'i opsiyonel yapmanın yarım kalmış tarafıydı.
12. **E2E altyapısı kırıkmış:** `jest-e2e.json` var olmayan `globalSetup` ve `globalTeardown` dosyalarına işaret ediyordu, `jest-e2e-setup.ts` ise `hrsync@localhost:5433/boilerplate_test` adresini hardcode ediyordu. Kurulum `.env.test` okuyacak şekilde yazıldı, veritabanı adının `_test` ile bitmesi zorunlu kılındı (geliştirme verisini korumak için) ve `jest-global-setup.ts` koşudan önce `prisma migrate deploy` çalıştırıyor.
13. **pg havuzu elle kapatılıyor.** `$disconnect()` `PrismaPg` adapter'ının havuzunu bırakmıyor ve Jest çıkmıyordu; `PrismaClientHost` ve test fixture'ı havuzu ayrı tutup kapatıyor.
14. **Faz 5'ten öne alınanlar:** `docker-compose.yml` çalışır hâle getirildi (var olmayan `.env.development` dosyasına bağımlıydı): `postgres:16-alpine` ve `redis:7-alpine` pinlendi, kimlikler Petzibu oldu, healthcheck eklendi, açılışta `petzibu_test` veritabanını kuran init script'i geldi. MinIO ve Mailpit hâlâ Faz 5'te.
15. **Puppeteer'ın Chrome'u kurulu değil.** Uygulama bunu zarifçe karşılıyor (log basıp PDF üretimini kapatıyor) ve açılış engellenmiyor. PDF üretimi E7'ye kadar gerekmiyor; `npx puppeteer browsers install chrome` ile çözülür.
16. **Silinen spec'ler:** `jwt.strategy.spec.ts` (telefon bazlı payload), `files.service.spec.ts` ve `files.controller.spec.ts` (sahiplik ve izin davranışı). İlk ikisinin yerine yeni modele göre 22 testlik bir `files.service.spec.ts` yazıldı.
17. **`uuid` paketi kaldırıldı**, yerine Node'un `crypto.randomUUID`'si geldi. Sebep: `uuid@14` yalnızca ESM ve pnpm'in `.pnpm/uuid@14/...` yolu jest'in `transformIgnorePatterns` desenine uymuyordu, bu yüzden e2e koşusu parse hatası veriyordu. Desen artık gerekmiyor ve iki jest yapılandırmasından da çıktı.
18. **Eski e2e spec'leri yeni hata modeline uyduruldu.** Altısı yalnızca `SentryExceptionFilter` kaydediyordu, o yüzden 404 bile 500 dönüyordu; ikisi de `ValidationPipe` kullanıyordu. Kurulum `main.ts` ile eşitlendi (iki filtre + `ZodValidationPipe`) ve beklentiler güncellendi: **bilinen HTTP hataları Sentry'ye gitmez**, yalnızca beklenmeyenler gider. Aksi hâlde her 404 gürültü olarak izlemeye düşerdi.

### Faz 4 · Auth ve işletme durumu — kapandı

Kapanış tarihi: 25 Eylül 2026. `pnpm lint | type-check | test | build` dördü de yeşil (API 36 suite / 490 test, mobil 8 suite / 55 test); `pnpm --filter @repo/api test:e2e` 8 suite / 118 test geçiyor, `auth.e2e-spec.ts` tek başına 32 iddia.

- [x] `argon2` kurulur; argon2id hash'leme gelir; `AES_SECRET_KEY` env'i kalkar *(dosya `hash.util.ts` değil `auth/services/password.service.ts`; aşağıdaki 1. maddeye bakın)*
- [x] `User`: `email` zorunlu unique, `passwordHash`, `role` enum (`owner | admin`), `businessId` nullable (admin için boş) *(şema Faz 3'te yazılmıştı; `passwordHash` artık doluyor)*
- [x] JWT payload: `sub`, `businessId`, `role`; `JwtStrategy` → CLS'e `businessId` yazar *(Faz 3'te geldi, burada e2e ile doğrulandı)*
- [x] Refresh token rotate ve hash'li saklama doğrulanır; çıkış ve şifre değişiminde hepsi silinir
- [x] Davet kabulü ve şifre sıfırlama: tek kullanımlık token, SHA-256 hash'li (§5.4)
- [x] `@Roles()` + `RolesGuard`
- [x] `TenantStatusGuard` global + `@AllowWhenSuspended()`
- [x] `GET /me`: işletme durumu + `pendingConsents[]` (legal modülü ile)
- [x] `ThrottlerModule` global; giriş ve sıfırlama route'larına limit *(public web uçları henüz yok)*
- [x] `@db.Date` / `@db.Time` alanları string'e çevrilir (§5.6) — Faz 3'ten taşındı *(serializer'da değil contracts primitiflerinde; 6. maddeye bakın)*
- [x] `mail` şablonları: davet, şifre sıfırlama, askı uyarısı; gönderim BullMQ kuyruğundan

#### Faz 4'te öğrenilenler ve plandan sapmalar

1. **`hash.util.ts` yeniden yazılmadı, `auth/services/password.service.ts` yazıldı.** ADR-0005 dosyanın *kaldırılmasını* söylüyor ve RULES.md `utils/` çöplüğünü yasaklıyor ("bir yardımcı ikinci kullanıcısı çıkana kadar tek kullanıcısının yanında durur"). Şifre hash'inin tek kullanıcısı auth modülüdür. `AES_SECRET_KEY` zaten Faz 1'de kalkmıştı, bu madde kendiliğinden kapalıydı.
2. **`JwtAuthGuard` global oldu ve controller'lardan `@UseGuards(JwtAuthGuard)` kalktı.** Tercih değil zorunluluk: Nest controller guard'larını global guard'lardan **sonra** çalıştırır, yani kimlik controller'da doğrulanırsa `TenantStatusGuard` ve `ConsentGuard` `request.user`'ı boş görür. Sıra `app.module.ts`'te: `JwtAuthGuard` → `RolesGuard` → `TenantStatusGuard` → `ConsentGuard`. `AppController`'ın kök route'u `@Public()` oldu.
3. **`businesses` modülü açıldı** (planda Faz 4 maddesi değildi, mimari doküman §4.1'de var). Hem `TenantStatusGuard` hem `GET /auth/me` işletme durumunu okumak zorunda ve `Business` tablosunun sahibi bu modül (§4.2). Bugün yalnızca okuyor; controller'ı yok, profil ve durum uçları kendi epic'leriyle gelecek.
4. **`legal` modülü açıldı:** metin okuma **public** (`GET /legal/texts/:type` — davet ve intake sayfaları oturumsuz gösterir), onay yazma owner'a ait (`POST /legal/consents`). Metin yayınlama (ADM-03) hâlâ `admin` modülünde ve yok; ilk sürümleri seed yayınlıyor.
5. **`ConsentGuard` eklendi.** §7.3'te tanımlıydı ama Faz 4 listesinde ayrı maddesi yoktu. Onay bekleyen owner `403 CONSENT_REQUIRED` alır; kilitlenmemesi için kimlik uçları ve onay uçları `@AllowWhenConsentPending()` taşır.
6. **Tarih dönüşümü serializer interceptor'ında değil contracts primitiflerinde yapıldı — ve Faz 3'ten kalan gerçek bir hatayı ortaya çıkardı.** `ZodSerializerInterceptor` yanıtı şemadan geçirir; şema `Date` kabul etmiyorsa araya girecek bir yer yok, o yüzden dönüşüm `isoDateTime`, `localDate` ve `localTime`'ın girişine kondu. Bu arada görüldü ki sorun `@db.Date`/`@db.Time` ile sınırlı değildi: `isoDateTime` de string beklediği için `userResponse.parse(user)` her `createdAt` değerinde patlıyordu, yani `GET /users/:id` ve `/users/me` **500 dönüyordu**. Üç primitif birlikte düzeltildi; kanıt `common/__tests__/prisma-date-serialization.spec.ts`.
7. **Boilerplate'in throttler'ı fiilen kapalıydı.** `@nestjs/throttler` v6'da `ttl` **milisaniyedir**; `auth.module.ts`'teki `ttl: 900` 15 dakika değil 900 ms demekti. Global tek limit (`DEFAULT_THROTTLE`, dakikada 120) kuruldu, kimlik uçları `@Throttle(AUTH_THROTTLE)` ile 15 dakikada 5 denemeye bağlandı. Adlandırılmış ikinci bir throttler kullanılmadı: global guard bütün adlandırılmış limitleri her route'a uygular, yani "yalnızca girişe sıkı limit" isteği route bazlı override ile çözülür.
8. **`ThrottlerException` İngilizce mesaj yazıyordu.** Filtre 429'u zaten `TOO_MANY_REQUESTS`'e eşliyordu ama gövdedeki `message` "ThrottlerException: Too Many Requests" oluyordu. Filtreye özel dal eklendi (§6.3: kullanıcıya dönen metin Türkçe).
9. **Mail kuyruğu açılışta bağlantının hazır olmasını bekler (`waitUntilReady()`).** Sebebi bir yarış: bağlantı hâlâ kurulurken uygulama kapanırsa bullmq `RedisConnection.close()` içinde `removeAllListeners()` çağırıyor, ardından yarıda kalan kurulum reddediliyor ve dinleyici kalmadığı için Node bunu **işlenmemiş hata** sayıp süreci düşürüyor. Uygulamayı açıp hemen kapatan e2e'ler (özellikle `swagger-setup`, AppModule'ü 10 kez kurar) tam bu yarışa denk geldi ve üç spec kırıldı. Ek olarak `MailQueue` ve `MailProcessor`'a `error` dinleyicileri eklendi, `app.e2e-spec.ts` artık uygulamayı kapatıyor. Bu, AppModule'e ilk gerçek BullMQ kuyruğunun girmesiyle ortaya çıktı.
10. **Seed artık şifre yazıyor ve yasal metinleri yayınlıyor.** `SEED_PASSWORD` (varsayılan `petzibu123`) ile admin ve owner giriş yapabilir; metinlerin v1'i ve seed owner'ının onayları da yazılır. Üründe kayıt ekranı yoktur (K9) ama seed bir geliştirme fikstürüdür: aksi hâlde `pnpm dev` sonrası hiçbir şekilde giriş yapılamıyordu.
11. **`users` CRUD `@Roles('admin')` oldu ve `POST /users` daveti kuyruğa alıyor.** MVP'de işletme başına tek kullanıcı vardır (K5), hesapları Petzibu ekibi açar (§7.4). Owner kendi ad ve soyadını `/users/me` üzerinden günceller.
12. **Kayıt uç noktası yok.** Plan "kayıt, giriş" diyordu; K9 gereği kayıt akışının yerini davet kabulü alıyor (`POST /auth/invite/accept`).
13. **`modules/auth/decorators/` silindi.** `public.decorator.ts` ve `current-user.decorator.ts` `common/decorators` altında birebir kopya hâlinde duruyordu (metadata anahtarı aynı olduğu için davranış hatası yoktu, yalnızca iki kaynak vardı).
14. **`test/utils/bootstrap.ts` eklendi.** E2E'ler global kurulumu (pipe, interceptor sırası, filtreler) tek yerden alıyor; her dosyanın `main.ts`'i kopyalaması interceptor sırası hatasını sessizce testlere taşıyordu.
15. **İki e2e dosyasında `listen` yarışı vardı.** `sentry-error-tracking` ve `health` spec'leri supertest'e **dinlemeyen** bir sunucu veriyordu; supertest o durumda her istekte kendisi `listen` eder ve eşzamanlı istekler (üç paralel `/error*`, üç paralel `/health*`) bu yarışta `ECONNRESET` alıyordu. İkisi de `await app.listen(0)` ile düzeltildi. Kalıntı bir flake'ti: kırılma her koşuda değil, arada bir oluyordu.
16. **Davet ve sıfırlama linkleri henüz bir sayfaya çıkmıyor.** `BASE_URL/davet/<token>` ve `BASE_URL/sifre-sifirlama/<token>` üretiliyor; SSR sayfaları `public-web` modülüyle gelecek (§10). Token'ın geçerliliğini soran uçlar (`GET /auth/invite/:token`, `GET /auth/reset-password/:token`) o sayfa için hazır.

### Faz 5 · Altyapı

- [ ] Kök `docker-compose.yml`: `postgres:16`, `redis:7`, `minio`, `mailpit`; sürümler pinli
- [ ] `apps/api/Dockerfile`: multi-stage, context repo kökü, `turbo prune @repo/api --docker`, pnpm; entrypoint `prisma migrate deploy && node dist/main`
- [ ] `.github/workflows/cd-production.yml` yeniden yazılır: pnpm, doğru context, `petzibu-api:<sha>` + `vX.Y.Z` etiketleri; deploy adımı hosting kararına kadar boş
- [ ] `.github/workflows/ci.yml`: `contracts` build adımı *(Node sürümü `.nvmrc`'den okunuyor, aşağıdaki maddeyle birlikte kapandı)*
- [ ] `.env.test`; `jest-e2e-setup.ts` hardcode'ları kalkar *(`.env.example` Faz 1'de yazıldı)*
- [x] Kökte `.nvmrc` ve `engines`; API ve mobil aynı Node sürümünü kullanır
- [x] `.husky/pre-commit`: lint + type-check *(Faz 1'de kapandı)*
- [x] `BULLMQ_PREFIX` varsayılanı `petzibu` *(Faz 1'de kapandı)*
- [x] `LoggingInterceptor` ve `LoggerService` gözden geçirilir (request id, `businessId` alanı)

#### Faz 5'te bugün kapanan iki madde

1. **Node sürümü tek yerden okunuyor.** `.nvmrc` `24`; `engines` kökte, `apps/api`'de ve `apps/mobile`'da aynı aralığı (`>=24.0.0 <25`) ve `pnpm`'i söylüyor — mobildeki `npm: >=11` kalıntısı kalktı (repo pnpm kullanıyor, ADR-0001). `ci.yml` artık sürümü sabit yazmıyor, `node-version-file: '.nvmrc'` ile okuyor; böylece CI Node 20'de, geliştirme 24'te kalmıyor. `engine-strict` **açılmadı**: bir bağımlılığın dar `engines` alanı kurulumu düşürebilir, uyarı yeterli. `cd-*.yml` ve `migration-dry-run.yml` hâlâ npm tabanlı ve Node 20'de; onlar bu fazın bekleyen deploy maddelerine ait.
2. **Log korelasyonu istek kimliğinden çıkıp bağlama taşındı.** Ayrıntı aşağıda.

#### Log gözden geçirmesinde bulunanlar

1. **İstek kimliği interceptor'da üretiliyordu, yani guard'da kesilen istek loglarda izlenemiyordu.** Nest sırası middleware → guard → interceptor; `401`/`403` yanıtları interceptor'a hiç gelmiyor, dolayısıyla ne `X-Request-ID` başlığı yazılıyor ne de filtrenin log satırı bir kimlik taşıyordu. Kimlik artık CLS middleware'inde üretiliyor (`database/cls-middleware.options.ts`): istemcinin `X-Request-ID` başlığı varsa korunur, yoksa UUID üretilir, yanıt başlığına yazılır. Kanıt: `logging-interceptor.e2e-spec.ts` — eşleşmeyen yolun `404`'ü de kimlik taşıyor.
2. **`businessId` hiçbir log satırında yoktu** (RULES.md "Kod kuralları" istiyor). `LoggerService` artık her satıra CLS'ten `requestId` ve `businessId` ekliyor (`common/logger/log-context.ts`); çağrı yerleri bu alanları elle geçirmez. Nest'in kendi logları da `app.useLogger` üzerinden aynı yoldan aktığı için filtre ve guard satırları da korelasyon taşıyor.
3. **Tek kullanımlık token'lar loga yazılıyordu.** `GET /auth/invite/:token` ve `GET /auth/reset-password/:token` sırrı **yolda** taşıyor; interceptor ve iki filtre ham `request.url` yazıyordu, Sentry'ye ayrıca `query` gidiyordu. `common/logger/redact-url.ts` route şablonundaki `:token` parametresini ve sırrı gösteren query anahtarlarını `[REDACTED]` yapıyor; üç çağrı yeri de oradan geçiyor.
4. **`logger.error(message, context)` çağrısı bağlamı yığın izi parametresine koyuyordu.** İmza `(message, trace?, context?)`; 5xx yanıtlarının bütün alanları (`statusCode`, `duration`, `url`) log satırının `stack` alanına düşüyordu. Düzeltildi ve teste bağlandı.
5. **`runAsTenant`/`runAsSystem` bağlamı sıfırlıyordu.** `cls.runWith` bütün store'u değiştirdiği için bir istek içinde sistem moduna geçildiğinde `requestId` kayboluyordu. İkisi de artık `cls.run({ ifNested: 'inherit' })` kullanıyor: tenant ezilir, kimlik korunur.
6. **Maskeleme listesine kişisel veri alanları eklendi** (`email`, `phone`, `fullName`, `address`, …). RULES.md aynı cümlede "kişisel veri girmez" diyor; liste son savunma hattı.
7. **Kapatılmayan bulgu:** `modules/mail` servisleri alıcı e-postasını log **mesajının içinde** yazıyor (`mail.service.ts:49,98`, `sendgrid.provider.ts:59`, `mail.processor.ts:53`). Alan adı değil düz metin olduğu için maskeleme listesi yakalamıyor. Mail modülünün kendi işinde temizlenecek; bu madde oraya taşındı.

### Faz 6 · Mobil taşıma — kapandı

Kapanış tarihi: 25 Eylül 2026. `pnpm lint | type-check | test | build` dördü de yeşil (mobilde 8 suite / 54 test). Mobilde tele giden şema kalmadı: hepsi `@repo/contracts`'tan geliyor.

- [x] `petzibu` reposu `apps/mobile` olarak taşınır (git geçmişi `git subtree` ile korunur)
- [x] npm → pnpm; `package-lock.json` silinir; kök `.npmrc`'ye `node-linker=hoisted` (Expo + pnpm için)
- [x] `metro.config.js` monorepo için doğrulanır — dosyaya dokunulmadı: `expo/metro-config` `pnpm-workspace.yaml`'ı okuyup `watchFolders`'a `packages/contracts`'ı kendisi ekliyor. ~~`packages/contracts` kaynaktan çözülür~~ → **dist'ten çözülür** (bkz. sapma 1)
- [x] `src/lib/schemas.ts` ve `src/features/*/schema.ts` → `@repo/contracts` import'ları. `lib/schemas.ts` silindi; `features/*/schema.ts` yalnızca form şemalarını tuttu, enum etiketleri `features/*/labels.ts`'e çıktı
- [x] `src/lib/api.ts` envelope tanımları `contracts/envelope.ts`'ten gelir (`unwrap`, `unwrapPaginated`, `errorBodyLoose`)
- [x] `mock-api/` silinir, `mock:api` script'i kalkar
- [x] `apps/mobile/docs/` silinir; epic'ler ve kaynaklar kök `docs/` altına taşınır (ADR-0008)
- [x] `RULES.md` "Not now" maddesi güncellenir: monorepo ve contracts artık var *(API sözleşmesi bölümü de baştan yazıldı)*
- [x] `CLAUDE.md` / `AGENTS.md` yolları monorepo'ya göre düzeltilir
- [x] Uygulama Expo Go'da açılıyor mu doğrulanır (K46) — `expo export --platform ios` Metro paketini üretiyor; giriş akışı gerçek API'ye karşı doğrulandı (bkz. sapma 3)
- [x] `turbo.json`'a `apps/mobile` için `lint`, `type-check`, `test` görevleri — görevler zaten ortaktı, eksik olan bağımlılıktı: `type-check`, `dev` ve `start` artık `^build`'e bağlı

#### Faz 6'da plandan sapmalar

1. **`@repo/contracts` mobile'a kaynaktan değil `dist`'ten çözülüyor.** Plan Metro'nun paketi `src`'den okumasıydı ve bunun için `exports`'a bir `react-native` koşulu eklendi. Çalışmadı: contracts `moduleResolution: nodenext` kullandığı için göreli import'ları `./primitives.js` yazmak **zorunda**, Metro ve jest ise bu yolu `.ts` dosyasına eşlemiyor. Uzantıları atmak API'nin type-check'ini kırardı. Bu yüzden `exports` `dist`'i gösteriyor ve sıralamayı turbo sağlıyor (`type-check`, `test`, `dev`, `start` → `^build`). Bedeli: tek başına `expo start` öncesi contracts bir kez derlenmeli.

2. **API'de modülü olmayan şemalar da contracts'a yazıldı.** `appointments`, `businesses`, `customers`, `pets`, `services`, `extras` şemaları bugün yalnızca mobil tarafından kullanılıyor; backend modülleri kendi epic'leriyle gelecek ve bu şemalara uyacak. Alternatif — şemaları mobilde bırakmak — iki kaynak demekti ve `features/*/schema.ts`'i "yalnızca ekran-özel türetme" kuralından çıkarırdı.

3. **Auth gerçek API'ye bağlandı; ekranların geri kalanı bağlanmadı.** Mock API silindiği için giriş, oturum (`GET /auth/me`), token yenileme ve çıkış gerçek uçlara gidiyor: `lib/auth-token.ts` artık token çiftini saklıyor, `lib/api.ts` `401`'de bir kez refresh deneyip isteği tekrarlıyor, kabuk (`_layout.tsx`, `TenantBanner`) `useBusiness` yerine `useMe`'den besleniyor. `customers`, `pets`, `services`, `extras` ve `business` ekranları karşılığı olmayan uçlara bakıyor ve epic'leri gelene kadar `404` alacak — bilinçli.

4. **Mobilin kendi husky/lint-staged kurulumu kaldırıldı.** Ayrı repodan kalmıştı; `prepare: husky` her `pnpm install`'da `.git can't be found` hatası veriyordu. Kök `.husky/pre-commit` zaten bütün paketler için lint + type-check çalıştırıyor.

5. **Faz 6'da `contracts`'ta iki küçük yapısal değişiklik yapıldı.** (a) `isoDateTime`, `localDate` ve `localTime`'ın yanına `…String` sürümleri eklendi: `z.preprocess` girdi tipini `unknown` yaptığı için form şemaları bu sarmalayıcıyı kullanamıyordu. (b) `ListQuery` ve türevleri `z.input` yerine `Partial<…Parsed>` oldu; `z.coerce` aynı sebeple sorgu dizesini kuran kodun tipini `unknown`'a düşürüyordu.

### Faz 7 · Docs yayını

Karar ADR-0008: kaynak bu repodaki `docs/` ve `RULES.md` dosyaları, `petzibu-docs` yalnızca yayın kabuğu. Kopyalama **çekme** yönünde: docs reposunun workflow'u bu repoyu checkout eder.

İçerik tarafı kapandı: `scripts/sync-docs.sh` iki ağacı eşitliyor, site `mkdocs build --strict` ile yeşil. Kalan iş otomasyon ve tokendır; o gelene kadar senkron elle çalıştırılır.

- [ ] `petzibu-docs/.github/workflows/deploy.yml`: ikinci bir `actions/checkout` (`oguzhanklss/petzibu`, `token: ${{ secrets.PETZIBU_REPO_TOKEN }}`, `path: source`) ve kopyalama adımı — `source/docs/**` → `docs/`, `source/apps/api/RULES.md` → `docs/rules-backend.md`, `source/apps/mobile/RULES.md` → `docs/rules-mobile.md`
- [ ] `PETZIBU_REPO_TOKEN`: kod reposuna yalnızca okuma yetkisi olan fine-grained PAT, docs reposunun secret'larına eklenir
- [x] `petzibu-docs`'taki içerik dosyaları kaynaktan üretilir hâle geldi: `docs/rules.md` silindi, yerine `rules-backend.md` + `rules-mobile.md` geldi; `docs/architecture/` (ADR'ler dahil) ve `docs/resources/` kaynaktan kopyalanıyor. `.gitignore`'a kopyalanan yolların girmesi **workflow kurulunca** yapılır: bugün içerik commit'li olmasa site boş çıkar
- [x] `docs/resources/` yolu: kaynak klasör `docs/resources/2026-09-23-pet-kuafor-rakip-analizi/` oldu ve `Petzibu – Rakip Analizi.md` → `petzibu-rakip-analizi-v1.md` olarak yeniden adlandırıldı. Site yolları zaten böyleydi; kopyalama adımının yol eşlemesine ihtiyacı yok
- [x] `mkdocs.yml` `nav`: `rules.md` → `rules-mobile.md` + `rules-backend.md`; `architecture/` altına ADR'ler ve bu belge. `mkdocs build --strict` yeşil ve gerçekten kontrol ediyor: uydurma bir bağlantıyla denendi, build kırıldı
- [x] `docs/index.md`: "Kaynak repo" notu tek monorepoyu ve "bu sitedeki dosyalar üretilmiştir, elle düzenlenmez" uyarısını söyler. `petzibu-docs/README.md` de aynı şeyi anlatır ve üretilen yolları listeler
- [x] Epic'lerdeki K47 ifadesi ("web repo'su") monorepo'ya göre güncellendi (`apps/web`). Epic'lerdeki `../../RULES.md` bağlantıları da düzeltildi: mobil repodan kalmışlar ve kod reposunda hiçbir yere gitmiyorlardı
- [x] `petzibu-docs`'ta bekleyen `mkdocs.yml` / `index.md` değişiklikleri bu işin içinde eridi (ikisi de baştan yazıldı)
- [ ] Bu belge de siteye çıkar; bütün maddeler kapanınca her iki yerden kalkar

---

## 3. Fazlar arası kontrol

| Faz sonu | Doğrulama |
| --- | --- |
| 1 ✅ | Kapandı 25 Eylül 2026. `lint`, `type-check`, `test` (34 suite / 486 test), `build` yeşil; `AppModule` yalnızca Config, Logger, Common, Queue, Prisma, Auth, Users, Files, Mail, DocumentGenerator, Health import ediyor |
| 2 ✅ | Kapandı 25 Eylül 2026. `contracts-pipeline.spec.ts` doğrulama + serileştirme + zarf + hata modelini uçtan uca kanıtlıyor; `users` modülü contracts şemalarıyla çalışıyor; `count` grep'i yalnızca Prisma `.count()` çağrılarını buluyor |
| 3 ✅ | Kapandı 25 Eylül 2026. `cross-tenant.e2e-spec.ts` 17 iddia: okuma, yazma, toplu işlem, bağlamsız sorgu ve `runAsSystem`. `enum-parity` kasıtlı uyuşmazlıkla doğrulandı. Seed çalışıyor, uygulama ayağa kalkıyor |
| 4 ✅ | Kapandı 25 Eylül 2026. `auth.e2e-spec.ts` 32 iddia: giriş (hesap varlığı sızmıyor), refresh rotate, çıkış, davet kabulü (tek kullanımlık), sıfırlama, şifre değişimi ve üç guard — askıdaki işletmede yazma `403 TENANT_SUSPENDED`, okuma çalışıyor; onaysız owner `403 CONSENT_REQUIRED` |
| 5 ⏸ | Node hizalaması ve log gözden geçirmesi kapandı (25 Eylül 2026): `X-Request-ID` guard'da kesilen istekte de var, log satırları `requestId` + `businessId` taşıyor, link token'ları maskeli; `logging-interceptor.e2e-spec.ts` 11 iddia. Kalanı hosting kararına bağlı: `docker compose up` sonrası `pnpm dev` ayağa kalkıyor, imaj build oluyor, CI yeşil |
| 6 ✅ | Kapandı 25 Eylül 2026. `expo export --platform ios` paketi üretiyor; `POST /auth/login`, `GET /auth/me`, `POST /auth/refresh` (rotate) ve hatalı girişin `401 UNAUTHORIZED` gövdesi çalışan API'ye karşı contracts şemalarıyla parse edildi. KUR-01'in kendisi (davet linki) web sayfasında açılır, mobilde iş çıkarmaz; mobil tarafın doğrulanan akışı KUR-02'dir |
| 7 | `mkdocs build --strict` yeşil; site epic'leri, mimariyi ve iki `RULES.md`'yi kaynak repodan çekerek yayımlıyor; docs reposunda içerik dosyası kalmıyor |
