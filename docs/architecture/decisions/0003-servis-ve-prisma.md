# ADR-0003: Modüller servis + doğrudan Prisma'dır; CQRS ve repository katmanı yoktur

Durum: Kabul edildi · Tarih: 2026-09-24

## Bağlam

Boilerplate iki örnek desen taşıyordu: `announcements` modülünde `@nestjs/cqrs` ile command/query/handler ayrımı, `users` modülünde `IUserRepository` arayüzü ve Prisma implementasyonu. Eski `CLAUDE.md` bunları bütün domain modülleri için zorunlu kılıyordu. Petzibu'da domain kuralları tek transaction içinde birkaç modülün servisini çağıran akışlardır (randevu tamamla → döküm oluştur; tahsilat → dökümlere dağıt). Bu akışlarda CQRS dosya sayısını üçe katlar, repository katmanı ise Prisma'nın zaten sağladığı soyutlamayı bir kez daha sarar. İkisinin de gerçek bir tüketicisi yoktu: ikinci bir ORM ya da ayrı read model planı yok.

## Karar

- Her modül `module.ts`, `controller.ts`, `service.ts` ve spec dosyasından oluşur. Servis Prisma'yı doğrudan kullanır.
- CQRS, in-process event bus (`@nestjs/event-emitter`) ve repository katmanı kullanılmaz. Modül sınırını aşan yan etki çağıran serviste açıkça yazılır.
- Modül gerçekten büyüdüğünde klasörlere ayrılır; önceden değil.
- Modüller arası sınır, kodla korunur: bir modül başka modülün tablolarına Prisma ile dokunmaz, yalnızca `exports`'taki servisini çağırır; döngü ve `forwardRef` yasaktır.

## Sonuçlar

- `announcements`, `@nestjs/cqrs`, `@nestjs/event-emitter`, `users/repositories` silinir.
- Test stratejisi buna göredir: saf kurallar unit test, geri kalanı gerçek Postgres'e karşı e2e. Servisler Prisma mock'lanarak test edilmez.
- Bir modül bir gün ayrı servise çıkarsa sınır zaten hazırdır: servis çağrısı HTTP çağrısına, ortak transaction event'e dönüşür.
