# ADR-0001: API, mobil ve web tek monorepoda yaşar

Durum: Kabul edildi · Tarih: 2026-09-24

## Bağlam

Mobil uygulama ayrı bir repoda (`petzibu`) npm ile geliştiriliyordu; backend boilerplate'i Turborepo + pnpm monorepo olarak geldi. Mobildeki `RULES.md` "Not now: monorepo, shared contract package" diyordu. Mimari doküman ise API sözleşmesinin zod şemalarıyla mobil ve backend arasında paylaşılmasını (ADR-0002) ve sözleşme değişikliğinin API ve istemci koduyla **aynı PR'da** yapılmasını istiyor. Bu iki isteğin ikisi de ayrı repolarda karşılanamaz: paylaşılan paket ya sürümlenmiş bir npm paketi ya da git submodule olur ve "aynı PR" kuralı bozulur.

Proje tek kişi tarafından geliştiriliyor. Polyrepo'nun asıl faydası olan "ayrı ekipler ayrı ritimle deploy eder" ihtiyacı yok.

## Karar

Tek monorepo: `apps/api`, `apps/mobile`, ileride `apps/web`; paylaşılan tek paket `packages/contracts`. Mobil repo `apps/mobile` altına taşınır, npm yerine pnpm kullanır. Landing site bu kararın dışındadır; API'ye bağımlı değildir ve yeri pilot bitince belirlenir.

## Sonuçlar

- Bir alan adı değiştiğinde API, şema ve mobil aynı commit'te kırılır ve düzelir; `type-check` bunu CI'da yakalar.
- Expo ve pnpm için `.npmrc`'de `node-linker=hoisted` gerekir; EAS Build monorepo köküne göre yapılandırılır.
- Mobil sürüm (mağaza) ve API deploy'u ayrı yaşam döngüsüne sahiptir; git tag adlandırması bunu ayırır (`mobile-vX.Y.Z`, `api-vX.Y.Z`).
- Mobil `RULES.md`'deki "Not now" maddesi ve K47'deki "web repo'su" ifadesi güncellenir.
- Bir gün ayrı ekipler oluşursa `apps/mobile`'ı monorepodan çıkarmak kolaydır; tersi zordur.
