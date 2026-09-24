# E6 · Hatırlatmalar

| Durum     | Faz | Platform        | Bağımlılık                                                 |
| --------- | --- | --------------- | ---------------------------------------------------------- |
| Yapılacak | MVP | Mobil öncelikli | [E4](04-takvim-randevu.md), [E5](05-randevu-operasyonu.md) |

> Story'ler: Kesinleşti v1.0 · 24 Eylül 2026

## Amaç

Gelmeyen müşteriyi azaltmak ve düzenli müşterinin tekrar gelmesini sağlamak.

## Nasıl çalışır

Uygulama hiçbir mesajı kendisi göndermez (K3). Her sabah owner'ın önünde bir **kuyruk** vardır: "Bugün gönderilecek 8 mesaj". Owner satıra dokunur, WhatsApp hazır mesajla açılır, gönderir, geri döner; satır kuyruktan düşer. Push yoktur (K46); kuyruk sekme rozetiyle görünür.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [README](index.md#alınan-kararlar).

| # | Karar |
| --- | --- |
| K3 | Hatırlatmalar yarı otomatik: kuyruk + tek dokunuşla WhatsApp. Şablonlar Faz 2'de otomatik gönderimde yeniden kullanılır. |
| K14 | Tek tıkla onay linki public web yüzeyinde açılır. |
| K20 | Arşivlenmiş hayvan hatırlatma sorgularına girmez. |
| K34 | İşletme Askıda iken kuyruk üretilmez. |
| K39 | Rebook hatırlatması müşteri bazındadır (`rebookReminderAt`). |
| K46 | Push bildirimi MVP'de yok. |
| K50 | Randevu hatırlatması **1 gün önce 18:00** gider; owner isteğe bağlı ikinci zaman olarak **aynı gün 09:00** açabilir. En fazla iki zaman. Bekliyor ve Onaylandı randevulara gider. |
| K51 | Kuyruktaki mesaj **WhatsApp açıldığı anda** "gönderildi" sayılır; geri alınabilir. |
| K52 | **Tek tıkla onay linki MVP'de.** Hesapsız public web sayfası; randevu başlayana kadar geçerli; tıklanınca randevu Onaylandı olur. |
| K53 | Ayrı aşı hatırlatma mesajı yok. Aşı durumu Bilinmiyor veya Süresi geçmiş ise randevu hatırlatmasına bir satır eklenir. |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

| ID     | Başlık                              | Platform    | Durum     |
| ------ | ----------------------------------- | ----------- | --------- |
| HAT-01 | Hatırlatma zamanları                | Mobil + Web | Yapılacak |
| HAT-02 | Mesaj şablonları                    | Mobil + Web | Yapılacak |
| HAT-03 | Gönderim kuyruğu                    | Mobil       | Yapılacak |
| HAT-04 | Randevu hatırlatması                | Mobil       | Yapılacak |
| HAT-05 | Tek tıkla onay sayfası              | Public web  | Yapılacak |
| HAT-06 | Rebook hatırlatması                 | Mobil       | Yapılacak |
| HAT-07 | Gelmeyenler listesi                 | Mobil + Web | Yapılacak |
| HAT-08 | "Hazır, alabilirsiniz" mesajı       | Mobil       | Yapılacak |

HAT-05 mobil uygulamada iş çıkarmaz; public web yüzeyinde (K14) ve backend'de yapılır. Burada duruyor çünkü HAT-04'ün ürettiği linke bağımlı.

---

## Ayarlar

**HAT-01 · Hatırlatma zamanları** · Mobil + Web
_Salon sahibi olarak randevu hatırlatmasının ne zaman kuyruğa düşeceğini belirlemek istiyorum._

- İki zaman vardır (K50):
  - **1 gün önce, 18:00** — varsayılan olarak açık, kapatılamaz.
  - **Aynı gün, 09:00** — varsayılan olarak kapalı, açılabilir.
- Saatler değiştirilebilir (15 dakikalık adımlar); zaman sayısı artırılamaz.
- Ayar değişince yalnızca henüz kuyruğa düşmemiş hatırlatmalar etkilenir.
- Gelmeyenler listesi eşiği de bu ekrandadır (HAT-07): varsayılan 8 hafta, 4–26 hafta arası.
**HAT-02 · Mesaj şablonları** · Mobil + Web
_Salon sahibi olarak müşteriye giden hazır mesajları kendi dilimle yazabilmek istiyorum._

- Şablon seti sabittir, her biri düzenlenebilir:
  1. Randevu hatırlatması (HAT-04)
  2. Rebook (HAT-06, HAT-07, OPR-04'teki rebook cümlesi)
  3. "Hazır, alabilirsiniz" (HAT-08)
  4. Bakım raporu mesajı (OPR-04)
  5. Borç hatırlatma (KAS-09)
  6. Form gönderme (INT-02)
- Şablonda yer tutucular kullanılır ve ekranda chip olarak eklenir: `{müşteri}`, `{hayvanlar}`, `{tarih}`, `{saat}`, `{salon}`, `{salon_telefon}`, `{onay_linki}`, `{form_linki}`, `{borç}`, `{hafta}`. Her şablonun kullanabileceği yer tutucular sınırlıdır; uygun olmayanı ekrana çıkmaz.
- Her şablonun "Varsayılana dön" butonu vardır. Varsayılan metinler Türkçe ve samimi dildedir.
- Kaydetmeden önce örnek verilerle önizleme gösterilir.
- Randevu hatırlatması şablonunda `{onay_linki}` zorunludur; kaldırılırsa kaydedilemez (K52).
- Şablonlar işletme bazındadır; ilk kullanımda varsayılanlar işletmeye kopyalanır.

---

## Kuyruk

**HAT-03 · Gönderim kuyruğu** · Mobil
_Salon sahibi olarak bugün göndermem gereken bütün mesajları tek listede görüp sırayla göndermek istiyorum._

- Kuyruk "Hatırlatmalar" sekmesindedir; bekleyen mesaj sayısı sekme rozetinde görünür (K46). Rozet INT-04 ile aynı mekanizmayla güncellenir.
- Her satırda: mesaj türü ikonu (randevu / rebook), müşteri adı, hayvan adları, bağlam ("Yarın 14:00" veya "Son ziyaret 6 hafta önce"), varsa "Onaylandı" etiketi (K50).
- Satıra dokununca WhatsApp, müşterinin numarası ve şablondan üretilmiş mesajla açılır. WhatsApp açıldığı anda satır **gönderildi** olarak işaretlenir ve kuyruktan düşer (K51).
- Kuyruğun altında "Bugün gönderilenler" bölümü kapalı halde durur; buradan bir satır "Geri al" ile kuyruğa döner.
- Satırı sola kaydırınca **"Atla"** çıkar: mesaj gönderilmeden kuyruktan düşer. Atlanan randevu hatırlatması bir daha kuyruğa girmez; atlanan rebook için müşterinin `rebookReminderAt` alanı temizlenir (müşteri gelmeyenler listesinde görünmeye devam eder, HAT-07).
- Kuyruk her açılışta sunucudan yeniden hesaplanır. Zamanı geçmiş satırlar (başlamış randevu, iptal edilmiş randevu) kendiliğinden düşer.
- İşletme Askıda ise kuyruk boştur ve "Hesabın askıda" şeridi görünür (K34).
- Web'de kuyruk yoktur; web'e yalnızca ayarlar (HAT-01, HAT-02) ve gelmeyenler listesi (HAT-07) girer.
**HAT-04 · Randevu hatırlatması** · Mobil
_Salon sahibi olarak yarınki randevular için müşterilere tek dokunuşla hatırlatma gönderebilmek istiyorum, böylece gelmeyen sayısı düşer._

- Bekliyor ve Onaylandı durumundaki randevular için, HAT-01'deki her açık zaman dilimi bir kuyruk satırı üretir (K50). Onaylandı randevunun satırı "Onaylandı" etiketiyle gelir; owner isterse atlar.
- Mesaj, "Randevu hatırlatması" şablonundan üretilir (HAT-02). `{onay_linki}` randevuya özel tek tıkla onay linkidir (HAT-05).
- Randevudaki herhangi bir hayvanın aşı durumu Bilinmiyor veya Süresi geçmiş ise (HAY-03) mesajın sonuna sabit bir satır eklenir: "Aşı karnesini getirmeyi unutmayın." (K53). Bu satır şablonda değil, üretimde eklenir.
- Aynı randevu için aynı zaman dilimi yalnızca bir kez kuyruğa girer. Randevunun tarihi değişirse (RAN-05) gönderilmemiş satırlar yeni tarihe göre yeniden hesaplanır; gönderilmiş olanlar kalır.
- İptal edilen randevunun satırları kuyruktan düşer (RAN-06).
- Randevu detayında (OPR-02) hatırlatma durumu görünür: "Hatırlatma gönderilmedi" / "Dün 18:05 gönderildi".
**HAT-05 · Tek tıkla onay sayfası** · Public web
_Pet sahibi olarak hatırlatma mesajındaki linke dokunup randevumu tek adımda onaylamak istiyorum._

- Link hesapsız açılır (K14). Sayfada salon adı, tarih, saat ve hayvan adları görünür; tek buton: "Randevumu onaylıyorum".
- Butona basınca randevu Onaylandı olur (OPR-01'deki `pending → confirmed` geçişi) ve "Teşekkürler, görüşmek üzere" ekranı gösterilir.
- Randevu zaten Onaylandı ise buton yerine "Randevunuz onaylı" yazar.
- Randevu iptal edilmiş, tamamlanmış, Gelmedi olmuş ya da başlangıç saati geçmişse link geçersizdir; "Bu link artık geçerli değil, salonu arayın: {salon_telefon}" mesajı gösterilir (K52).
- İşletme Askıda ise aynı geçersiz mesajı gösterir (K34).
- Sayfa yalnızca onaylar; iptal, erteleme veya not bırakma yoktur.

---

## Rebook ve gelmeyenler

**HAT-06 · Rebook hatırlatması** · Mobil
_Salon sahibi olarak "şimdi değil" diyen müşteriye zamanı gelince tek dokunuşla "yer ayıralım mı?" mesajı gönderebilmek istiyorum._

- `rebookReminderAt` bugüne eşit veya geçmiş olan müşteriler için kuyruğa bir rebook satırı düşer (K39).
- Müşterinin ileri tarihli sonuçlanmamış randevusu varsa satır üretilmez; `rebookReminderAt` zaten OPR-06 kuralıyla geçersizdir.
- Mesaj "Rebook" şablonundan üretilir; `{hayvanlar}` müşterinin arşivlenmemiş hayvanlarıdır, `{hafta}` OPR-06'da seçilen aralıktır.
- Gönderilince `rebookReminderAt` temizlenir; müşteri yeniden gelmezse gelmeyenler listesi (HAT-07) devreye girer. Aynı müşteriye ikinci bir rebook mesajı otomatik üretilmez.
- Hatırlatma gönderildikten sonra müşteri detayında "Rebook hatırlatması gönderildi · 12 Eyl" bilgisi görünür.
**HAT-07 · Gelmeyenler listesi** · Mobil + Web
_Salon sahibi olarak uzun süredir gelmeyen müşterileri görmek ve onlara tek dokunuşla ulaşmak istiyorum._

- Liste "Hatırlatmalar" sekmesinin altında ayrı bir bölümdür; web'de kendi sayfasıdır.
- Kural: son tamamlanan randevusu (`lastVisitAt`) eşikten eski (HAT-01, varsayılan 8 hafta), ileri tarihli sonuçlanmamış randevusu yok, en az bir arşivlenmemiş hayvanı var. `lastVisitAt` boş olan (hiç gelmemiş) müşteriler listeye girmez.
- Rebook hatırlatması kaydedilmiş veya gönderilmiş olması müşteriyi listeden çıkarmaz.
- En uzun süredir gelmeyen en üstte; her satırda müşteri adı, hayvan adları, "9 haftadır gelmedi".
- Satırdaki "Mesaj" butonu WhatsApp'ı "Rebook" şablonuyla açar; `{hafta}` boş geçer ve şablon bu durumda "yakında" der. Bu gönderim kuyruğa girmez, durum tutulmaz.
- Satırdaki "Randevu" butonu RAN-02'yi müşteri seçili açar.
- Satır sola kaydırılıp "Gizle" ile listeden çıkarılabilir; müşteri tekrar gelene kadar bir daha görünmez.
**HAT-08 · "Hazır, alabilirsiniz" mesajı** · Mobil
_Salon sahibi olarak bakım bitince müşteriye tek dokunuşla "hazır" mesajı göndermek istiyorum._

- Randevu detayındaki her hayvan kartında (OPR-02) "Hazır" butonu bulunur; yalnızca Geldi durumunda görünür.
- Butona basınca WhatsApp "Hazır, alabilirsiniz" şablonuyla açılır; `{hayvanlar}` yalnızca o hayvandır.
- Durum tutulmaz, kuyruğa girmez; buton tekrar basılabilir.

---

## Kapsam dışı

- Push bildirimi (K46, Faz 2)
- Otomatik WhatsApp gönderimi ve teslim durumu takibi (Faz 2)
- Ayrı aşı hatırlatma mesajı (K53)
- SMS ve e-posta ile hatırlatma
- İkiden fazla hatırlatma zamanı
- Onay sayfasından iptal veya erteleme
- Pazarlama kampanyaları ve toplu mesaj (Faz 3, İYS)
- Serbest şablon ekleme; yalnızca sabit set düzenlenir

## Teknik notlar

| Konu | Karar | Story |
| --- | --- | --- |
| Ayar modeli | `business.reminderSlots`: `[{ offsetDays: 1, time: "18:00" }, { offsetDays: 0, time: "09:00", enabled: false }]` biçiminde iki sabit giriş; `business.inactiveWeeks` (varsayılan 8). Saatler Europe/Istanbul'a göredir. | HAT-01, HAT-07 |
| Şablon modeli | `messageTemplate` (`businessId`, `type: appointment \| rebook \| ready \| report \| debt \| intake`, `body`). Varsayılanlar backend'de sabittir; işletme kaydı yoksa varsayılan döner, "Varsayılana dön" işletme kaydını siler. Yer tutucu doğrulaması (zorunlu `{onay_linki}`, izin verilmeyen yer tutucu) API'de `422 VALIDATION_FAILED`. | HAT-02 |
| Mesaj üretimi | Şablondan mesaj üretimi yalnızca **backend'de** yapılır: `GET /reminders/queue` satırla birlikte hazır `message` döner; OPR-04, KAS-09, INT-02 ve HAT-07/08 de kendi mesajlarını aynı servisten alır. İstemci şablon çözmez. Aşı satırı (K53) bu serviste eklenir. | Tümü |
| Kuyruk hesabı | Kuyruk saklanmaz; `GET /reminders/queue` her çağrıda hesaplar. Gönderim durumu saklanır: `reminderLog` (`type`, `appointmentId` veya `customerId`, `slot` nullable, `sentAt`, `skippedAt`). Bir satır kuyruğa girer ancak `reminderLog`'da eşleşen kayıt yoksa. "Geri al" ilgili log kaydını siler. | HAT-03, HAT-04, HAT-06 |
| Kuyruk süzgeci | Randevu satırı: `status in (pending, confirmed)`, `startAt` gelecekte, slot zamanı geçmiş ya da bugün. Rebook satırı: `rebookReminderAt <= bugün`, ileri tarihli sonuçlanmamış randevu yok, arşivlenmemiş hayvan var. Askıda işletme için boş liste. | HAT-03 |
| Rozet | `GET /reminders/queue/count`; INT-04'teki count uç noktasıyla aynı desende. | HAT-03 |
| Onay linki | Randevuda `confirmToken` (rastgele, benzersiz). `GET /confirm/:token` public web sayfası; `POST` `pending → confirmed` geçişini OPR-01'deki durum servisiyle yapar, ayrı bir yol yoktur. `startAt` geçmişse veya durum uygun değilse `410 LINK_EXPIRED`. Rate limit INT-03 ile aynı. | HAT-05 |
| WhatsApp açma | MUS-06 ile aynı `whatsapp://send?phone=&text=` şeması; metin URL-encode edilir. Uygulama geri planda kaldığında (WhatsApp açıldı) `POST /reminders/:id/sent` gönderilir; yanıt beklenmez, hata olursa satır bir sonraki açılışta yeniden görünür. | HAT-03 |
| Gelmeyenler | `GET /customers?inactive=true`: MUS-01 liste uç noktasına filtre. `customer.hiddenFromInactiveAt`; tamamlanan randevu bu alanı boşaltır (OPR-05). | HAT-07 |
| Rebook temizleme | `POST /reminders/:id/sent` rebook satırı için `rebookReminderAt` alanını boşaltır ve `customer.rebookRemindedAt` yazar. | HAT-06 |
| Hazır butonu | OPR-02 hayvan kartına eklenir; mesajı `GET /appointments/:id/pets/:petId/ready-message` döner. | HAT-08 |
| Kod yeri | `features/reminders/` (queue, settings, templates, inactive). Sekme `app/(tabs)/reminders.tsx` yalnızca kompozisyon. Public onay sayfası backend projesinde. | Tümü |

## Diğer epic'lere bağlantılar

| Buradan | Oraya | Konu |
| --- | --- | --- |
| HAT-02 | OPR-04, KAS-09, INT-02 | Rapor, borç ve form mesajlarının şablonları buradan gelir |
| HAT-03 | INT-04 | Rozet mekanizması |
| HAT-04 | HAY-03 | Aşı satırı |
| HAT-04 | RAN-05, RAN-06 | Tarih değişince yeniden hesap, iptalde düşme |
| HAT-04 | OPR-02 | Randevu detayında hatırlatma durumu |
| HAT-05 | OPR-01 | `pending → confirmed` geçişi |
| HAT-05 | K14 | Public web yüzeyi |
| HAT-06 | OPR-06, HAY-05 | `rebookReminderAt` yazılması ve temizlenmesi |
| HAT-07 | MUS-01, RAN-02 | Liste uç noktası ve randevu formu |
| HAT-08 | OPR-02 | Hayvan kartındaki "Hazır" butonu |
| — | ADM-02 | Askıda kuyruk boş |

## Açık sorular

- Yok.
