# E8 · Hesap & Veri

| Durum     | Faz | Platform    | Bağımlılık                  |
| --------- | --- | ----------- | --------------------------- |
| Yapılacak | MVP | Mobil + Web | [E1](01-kurulum-isletme.md) |

## Amaç

Güven vermek ve yasal gerekliliklere uymak.

## Kapsam

- [ ] KVKK aydınlatma metni
- [ ] Müşteri silme veya anonimleştirme
- [ ] Veriyi dışa aktarma (müşteriler, randevular, kasa)
- [ ] Bildirim, tema ve dil ayarları
- [ ] Çıkış yapma
- [ ] Hesap silme (App Store ve Play Store bunu zorunlu tutuyor)

## Kapsam dışı

- —

## İlgili kararlar

- K34 (askıda salt okunur, 12 ay sonra silme), K35 (veri sorumlusu ayrımı). Bkz. [README](index.md#alınan-kararlar).

## Story'ler

_Henüz yazılmadı. Format için bkz. [README](index.md#story-formatı)._

## Rakip analizinden gelen kriterler

Story'ler yazılırken işlenecek. Kaynak: rakip analizi v2, §6 E8.

- **Veri erişim garantisi (K34):** abonelik bitse de hesap Askıda kalır: kayıtlar görüntülenir ve aranır, ara/WhatsApp butonları çalışır, veri dışa aktarılabilir, KVKK işlemleri (müşteri silme/anonimleştirme, hesap silme) yapılabilir. Yazma kapalıdır. Ayrıntı ADM-02'de.
- **Veri silme takvimi (K34):** Askıda en fazla 12 ay; silmeden 30 gün önce owner'a haber verilir.
- **İptal talebi:** ayarlarda "Aboneliği iptal et" talebi butonu; K10 gereği ödeme uygulama dışında olduğu için buton Petzibu ile WhatsApp veya e-posta iletişimi açar. Uygulama içinde ödeme veya iptal işlemi yoktur.
- **KVKK ve onam metinleri sürümlü:** aydınlatma metni ve bakım riski onayı (INT-03) sürüm numarasıyla tutulur; müşteri kaydında hangi sürümün ne zaman onaylandığı saklanır.
- **Veri sorumlusu ayrımı (K35):** üç metin tanımlanır. (1) Owner aydınlatma metni: veri sorumlusu Petzibu. (2) Veri işleme sözleşmesi: salon veri sorumlusu, Petzibu veri işleyen; owner KUR-01'de onaylar. (3) Müşteri aydınlatma metni: işletme bazlı şablon, salon adı ve adresi otomatik dolar; intake formunda (INT-03) gösterilir. Her metin sürümlüdür.

## Açık sorular

-
