# Petzibu Docs

Petzibu, Türkiye'deki pet kuaförü salonları için mobil öncelikli, çok kiracılı bir SaaS'tır. Bu site ürün ve teknik dokümantasyonu toplar: epic'ler, alınan kararlar, mimari, kod kuralları ve araştırma kaynakları.

## Bölümler

- **[Epic Haritası](epics/index.md)** — MVP kapsamı, epic'lerin durumu, bağımlılıklar ve ürün kararları (K1…K58). Story'ler her epic'in kendi sayfasında.
- **[Backend Mimari Dokümanı](architecture/backend-architecture.md)** — Modüler monolit, tenant izolasyonu, API sözleşmesi, kimlik doğrulama, arka plan işleri.
- **[Kararlar (ADR)](architecture/decisions/index.md)** — Kodun şeklini belirleyen, tartışması kapanmış teknik kararlar.
- **[Kod Kuralları](rules-backend.md)** — Backend ve [mobil](rules-mobile.md) için "nasıl yazılır" kuralları.
- **[Kaynaklar](resources/2026-09-23-pet-kuafor-rakip-analizi/petzibu-rakip-analizi-v2.md)** — Rakip analizi ve pazar araştırması.

!!! warning "Bu sayfalar üretilmiştir"
    Dokümanların tek kaynağı kod reposudur (`oguzhanklss/petzibu`): `docs/` ağacı ve uygulama kökündeki `RULES.md` dosyaları. Bu repo yalnızca yayın kabuğudur — buradaki içerik dosyalarını elle düzenleme, bir sonraki kopyalamada kaybolur. Bir kararı değiştiren PR dokümanı da taşır (ADR-0008).

