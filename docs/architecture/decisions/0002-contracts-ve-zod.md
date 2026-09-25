# ADR-0002: API sözleşmesi `packages/contracts` içinde zod şemalarıdır

Durum: Kabul edildi · Tarih: 2026-09-24

## Bağlam

Mobil uygulama her yanıtı zod şemasıyla parse ediyor ve formları aynı şemalarla doğruluyor (`features/*/schema.ts`, `lib/schemas.ts`). Boilerplate ise class-validator + class-transformer DTO sınıfları ve Joi env doğrulaması kullanıyor. İki taraf aynı şekli iki kez, iki farklı dille yazıyordu ve sapma kaçınılmazdı.

## Karar

- Sözleşmenin tek kaynağı `packages/contracts`: primitives (id, para, tarih, telefon), envelope, hata kodları, enum'lar ve modül başına request/response şemaları. Zod 4.
- Backend `nestjs-zod` ile doğrular (`ZodValidationPipe` global) ve serialize eder (`ZodSerializerInterceptor`). DTO sınıfı yazılmaz; Swagger aynı şemalardan üretilir.
- Env doğrulaması da zod'dur; Joi çıkar. Tek doğrulama kütüphanesi.
- Enum'ların kaynağı `contracts`'tır. Prisma enum'larıyla eşitlik `apps/api/src/database/enum-parity.ts` içinde tip seviyesinde kontrol edilir.
- Route tanımları pakete girmez; controller'lar Nest'te kalır. Client codegen yoktur.

## Sonuçlar

- Response serializer şemada olmayan alanı dışarı çıkarmaz; `passwordHash` gibi alanların sızması yapısal olarak imkânsızdır.
- `contracts` derlenir (`tsup`, ESM + CJS + d.ts) ve Turborepo `^build` bağımlılığıyla önce üretilir. Metro kaynaktan da çözebilir.
- `contracts` Prisma'ya bağımlı olamaz; mobil onu import eder.
- Boilerplate'in `common/base/dto`, `common/swagger`, Postman üretici ve bütün `dto/` klasörleri silinir.
- Sözleşme değişikliği onu kullanan API ve istemci koduyla aynı PR'da yapılır; sürümleme yoktur (ADR-0001).
