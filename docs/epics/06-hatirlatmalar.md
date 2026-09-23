# E6 · Hatırlatmalar

| Durum     | Faz | Platform        | Bağımlılık                                                 |
| --------- | --- | --------------- | ---------------------------------------------------------- |
| Yapılacak | MVP | Mobil öncelikli | [E4](04-takvim-randevu.md), [E5](05-randevu-operasyonu.md) |

## Amaç

Gelmeyen müşteriyi azaltmak ve düzenli müşterinin tekrar gelmesini sağlamak.

## Kapsam

- [ ] Randevu hatırlatması
- [ ] Rebook hatırlatması
- [ ] Aşı süresi dolma hatırlatması
- [ ] Yarı otomatik gönderim: uygulama bildirim gönderir, owner tek dokunuşla hazır WhatsApp mesajını açar
- [ ] Düzenlenebilir mesaj şablonları
- [ ] Hatırlatma zamanlaması ayarı (ör. randevudan 1 gün önce)

## Kapsam dışı

- Otomatik WhatsApp gönderimi (Faz 2)
- SMS
- Pazarlama kampanyaları

## İlgili kararlar

- K3 (yarı otomatik hatırlatma; şablonlar Faz 2'de yeniden kullanılır). Bkz. [README](index.md#alınan-kararlar).

## Story'ler

_Henüz yazılmadı. Format için bkz. [README](index.md#story-formatı)._

## Rakip analizinden gelen kriterler

Story'ler yazılırken işlenecek. Kaynak: rakip analizi v2, §6 E6.

- **Tek tıkla onay linki:** hatırlatma mesajında hesapsız bir onay linki (K14 public web yüzeyi). Müşteri tıklayınca randevu `confirmed` (Onaylandı) olur. Bu, randevunun Onaylandı durumuna geçtiği tek yol olabilir; owner da elle onaylayabilir (E5).
- **Toplu hatırlatma kuyruğu:** tek tek bildirim yerine "Bugün gönderilecek 8 mesaj" ekranı. Owner sırayla aç-gönder yapar. Push olmadan çalışır; push gelince yalnızca "kuyrukta N mesaj var" bildirimi atar.
- **Gelmeyenler listesi:** "X haftadır gelmeyenler" (`lastVisitAt` üzerinden; süre ayarlanabilir). Rebook tarihi kaydedilmemiş müşterileri de kapsar. Satırdan tek dokunuşla rebook şablonu açılır.
- **Birden fazla hatırlatma zamanı:** zamanlama ayarı birden fazla girişe izin verir (ör. 1 gün önce ve aynı sabah).
- **Aşı satırı:** hayvanın aşı durumu "Bilinmiyor" veya "Süresi geçmiş" ise hatırlatma mesajına "aşı karnesini getirmeyi unutmayın" satırı eklenir.
- **Askıda durur (K34):** işletme Askıda iken hatırlatma kuyruğu üretilmez ve bildirim gitmez.
- **Şablon seti:** randevu, rebook, aşı, "hazır, alabilirsiniz" (E5), rapor mesajı (E5), borç hatırlatma (KAS-09). Hepsi düzenlenebilir.
- **Sonra (Faz 2):** otomatik gönderimde başarısız mesajı tekrar deneme kuyruğu; WhatsApp iletilmezse SMS'e düşme.

## Açık sorular

-
