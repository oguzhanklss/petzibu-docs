# Mimari Karar Kayıtları (ADR)

Mimari dokümanın gerekçesini taşıyan, bir kez verilmiş ve tartışması kapanmış kararlar. Her kayıt kısa tutulur: bağlam, karar, sonuçlar. Bir karar değişirse eski kayıt silinmez, `Durum` alanı "Yerini X aldı" olur ve yeni kayıt açılır.

Ürün kararları (K1…K62) burada değil, epic dosyalarında yaşar. Buraya yalnızca kodun şeklini belirleyen teknik kararlar girer.

| # | Karar | Durum |
| --- | --- | --- |
| [0001](0001-tek-monorepo.md) | API, mobil ve web tek monorepoda yaşar | Kabul edildi |
| [0002](0002-contracts-ve-zod.md) | API sözleşmesi `packages/contracts` içinde zod şemalarıdır; DTO sınıfı yazılmaz | Kabul edildi |
| [0003](0003-servis-ve-prisma.md) | Modüller servis + doğrudan Prisma'dır; CQRS ve repository katmanı yoktur | Kabul edildi |
| [0004](0004-tenant-extension.md) | Tenant izolasyonu Prisma extension ve CLS ile zorlanır | Kabul edildi |
| [0005](0005-argon2id.md) | Şifreler argon2id ile hash'lenir | Kabul edildi |
| [0006](0006-i18n-yok.md) | Backend'de i18n yoktur; mesajlar Türkçe ve inline'dır | Kabul edildi |
| [0007](0007-uuid-v7.md) | Birincil anahtarlar UUID v7'dir | Kabul edildi |
| [0008](0008-docs-kaynagi-kod-reposu.md) | Dokümanların kaynağı kod reposudur; site onu build sırasında kopyalar | Kabul edildi |
| [0009](0009-uydu-tablo-ayri-modul-degil.md) | Uydu tablo ayrı modül değildir, aynı modülde ikinci servistir | Kabul edildi |
| [0010](0010-dokum-ara-toplami-ve-statements-orkestrasyonu.md) | Döküm ara toplamı `billing`'de saklanır; döküm uçları `statements` orkestrasyonundadır | Kabul edildi |

## Biçim

```markdown
# ADR-NNNN: Başlık

Durum: Kabul edildi · Tarih: YYYY-MM-DD

## Bağlam
## Karar
## Sonuçlar
```
