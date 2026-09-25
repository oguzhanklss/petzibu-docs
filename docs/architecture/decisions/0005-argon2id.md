# ADR-0005: Şifreler argon2id ile hash'lenir

Durum: Kabul edildi · Tarih: 2026-09-24

## Bağlam

Boilerplate'in `hash.util.ts` dosyası şifreleri AES-256-GCM ile **şifreliyordu**: `AES_SECRET_KEY` bilinirse her şifre geri çözülebilir. Bu bir hash değildir ve veritabanı ile anahtar birlikte sızarsa bütün şifreler açığa çıkar. Son commit bcrypt'i kaldırmış, mimari dokümanın ilk sürümü ise bcrypt yazıyordu.

## Karar

- Şifreler `argon2` paketiyle **argon2id** olarak hash'lenir; paketin varsayılan parametreleri kullanılır (OWASP önerisiyle uyumlu). Doğrulama `argon2.verify`.
- `AES_SECRET_KEY` env değişkeni ve `hash.util.ts` kaldırılır.
- Yüksek entropili token'lar (refresh token, davet ve sıfırlama token'ları) için argon2 gerekmez; SHA-256 yeterlidir ve hızlıdır. Bu ayrım mimari doküman §5.4'te tanımlıdır.

## Sonuçlar

- bcrypt yerine argon2id: 72 bayt sınırı yok, bellek maliyetli, GPU saldırılarına dirençli. Native modül olduğu için Docker imajında build araçları build aşamasında bulunur.
- Hash'in ilk `$argon2id$` öneki sürüm bilgisini taşır; parametre değişirse eski hash'ler giriş anında yeniden hash'lenebilir.
