# E5 · Randevu Operasyonu

| Durum | Faz | Platform | Bağımlılık |
| --- | --- | --- | --- |
| Yapılacak | MVP | Mobil öncelikli | [E4](04-takvim-randevu.md) |

> Story'ler: Kesinleşti v1.1 · 23 Eylül 2026

## Amaç

Müşterinin salona gelişinden bakımın bitişine kadar olan süreç akıcı olsun.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [README](index.md#alınan-kararlar).

| # | Karar |
| --- | --- |
| K1 | Randevu içinde birden fazla hayvan olabilir; her hayvanın kendi hizmet satırları ve bakım raporu vardır. |
| K2 | Tamamlama gelir oluşturmaz; gelir yalnızca tahsilatta oluşur (E7). |
| K3 | Bakım raporu ve mesajlar yarı otomatik gönderilir (paylaşım menüsü / WhatsApp). |
| K26 | Geçmiş tarihli randevu girilebilir. |
| K32 | Satır fiyatı: hayvan + hizmet özel fiyatı varsa o, yoksa kademe fiyatı. |
| K33 | Önce/sonra kolajı telefonda, paylaşım anında üretilir; saklanmaz. |
| K37 | Tamamlanan randevu **geri alınamaz**. Tutar ve kalem düzeltmeleri E7'deki döküm üzerinden yapılır. |
| K38 | "Geldi" adımı **atlanabilir**: Bekliyor veya Onaylandı durumundaki randevu doğrudan tamamlanabilir (geçmiş tarihli randevular dahil). |
| K39 | Rebook hatırlatması **müşteri bazındadır**; hayvan bazında değildir. |
| K40 | Bakım raporu tamamlama akışından **bağımsızdır**; zorunlu bir adım değildir. |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

| ID | Başlık | Platform | Durum |
| --- | --- | --- | --- |
| OPR-01 | Durum akışı | Mobil + Web | Yapılacak |
| OPR-02 | Randevu detayı | Mobil + Web | Yapılacak |
| OPR-03 | İşlem sırasında hizmet ve ek ücret ekleme | Mobil | Yapılacak |
| OPR-04 | Bakım raporu | Mobil | Yapılacak |
| OPR-05 | Tamamlama | Mobil + Web | Yapılacak |
| OPR-06 | Sonraki randevu | Mobil + Web | Yapılacak |

---

## Durum ve detay

**OPR-01 · Durum akışı** · Mobil + Web
*Salon sahibi olarak randevunun hangi aşamada olduğunu tek dokunuşla güncelleyebilmek istiyorum.*
- Durumlar ve renkleri:
  - Bekliyor (gri)
  - Onaylandı (gri + tik)
  - Geldi (amber #E8C468)
  - Tamamlandı (teal #2A9D90)
  - İptal (gri + üstü çizili; E4, RAN-06)
  - Gelmedi (kırmızı #EF4444)
- İzin verilen geçişler:

  | Durumdan | Geçebileceği durumlar |
  | --- | --- |
  | Bekliyor | Onaylandı, Geldi, Tamamlandı, Gelmedi, İptal |
  | Onaylandı | Geldi, Tamamlandı, Gelmedi, İptal |
  | Geldi | Tamamlandı, İptal |
  | Gelmedi | Bekliyor (geri alma) |
  | Tamamlandı | — (K37) |
  | İptal | — (RAN-06) |

- "Onaylandı" elle işaretlenir; müşteri hatırlatmaya olumlu cevap verdiğinde owner bu durumu seçer. E6'daki tek tıkla onay linki de aynı durumu üretir.
- "Geldi" adımı atlanabilir (K38).
- Randevunun bitiş saati geçmiş ve durumu hâlâ Bekliyor veya Onaylandı ise, takvim bloğunda ve randevu detayında **"Geldi mi?"** sorusu gösterilir: "Tamamlandı" / "Gelmedi". Bu soru geçmiş tarihli yeni randevular için de gösterilir (RAN-02).
- "Gelmedi" olarak işaretlenen randevu müşterinin gelmedi sayısına (MUS-04) yansır ve takvimde görünür kalır (K27).
- "Gelmedi" geri alınırsa randevu Bekliyor durumuna döner ve gelmedi sayısından düşer.
- "Geldi" durumuna geçerken ek bir bilgi istenmez.

**OPR-02 · Randevu detayı** · Mobil + Web
*Salon sahibi olarak randevuyu açtığımda, işe başlamadan bilmem gereken her şeyi tek ekranda görmek istiyorum.*
- **Üst bölüm:** durum chip'i, tarih ve saat aralığı, duruma göre değişen ana aksiyon butonu:
  - Bekliyor / Onaylandı → "Geldi" (ikincil: "Onaylandı" işaretle)
  - Geldi → "Tamamla"
  - Tamamlandı → "Dökümü gör" (E7)
- **Müşteri kartı:**
  - Ad ve telefon; ara ve WhatsApp butonları (MUS-06)
  - Müşteri etiketleri (MUS-05)
  - Risk chip'leri: borç, gelmedi sayısı, yaklaşan randevu sayısı. Yalnızca değer sıfırdan büyükse görünür. Borç chip'i E7 hazır olunca görünür.
  - Müşteri bakım riski onayını (INT-03) hiç vermemişse "Bakım onayı yok" işareti; dokununca "Formu gönder" (INT-02) açılır. MUS-04 ile aynı işaret.
- **Uyarı şeritleri:** çakışma (RAN-04), aşı sorunu (HAY-03), hayvan uyarı etiketleri (HAY-02).
- **Her hayvan için ayrı kart:**
  - Fotoğraf, ad, ırk, boyut, uyarı ikonları
  - Hizmet satırları (süre ve fiyat) ve ek ücretler
  - **Son ziyaret:** hayvanın son tamamlanan randevusundaki rapor notu ve "sonra" fotoğrafı; varsa referans fotoğrafı (HAY-04) yanında. Geçmiş yoksa bölüm görünmez.
  - Bakım raporu durumu: "Rapor yok" / "Hazır" / "Paylaşıldı"
- **Ziyarete özel not** (RAN-07).
- **Alt bölüm:** toplam tutar.
- **"⋯" menüsü:** düzenle (RAN-05), iptal (RAN-06), formu gönder (INT-02). Sonuçlanmış randevuda düzenle ve iptal görünmez.

---

## İşlem sırasında

**OPR-03 · İşlem sırasında hizmet ve ek ücret ekleme** · Mobil
*Salon sahibi olarak işe başladıktan sonra bir hayvana "keçe açma" gibi bir ek ücret veya ek hizmet ekleyebilmek istiyorum, böylece eklediğim kalem dökümde otomatik görünür.*
- Randevu Bekliyor, Onaylandı veya Geldi durumundayken yapılabilir.
- Ek ücret, KUR-08'de tanımlı kalemlerden seçilir; tutarı o an değiştirilebilir.
- Ek ücret hayvan bazında eklenir ("Paşa: Keçe açma 150 ₺").
- Ek ücret randevunun süresini değiştirmez. Ek hizmet (RAN-05 ile aynı kural) süreyi ve bitiş saatini değiştirir; fiyatı K32 kuralıyla gelir.
- Ek ücretin adı ve tutarı randevuya o anki değerleriyle kopyalanır.
- Eklenen kalem kaldırılabilir.

**OPR-04 · Bakım raporu** · Mobil
*Salon sahibi olarak her hayvan için önce/sonra fotoğraflı bir rapor oluşturup WhatsApp'tan göndermek istiyorum.*
- Rapor hayvan başınadır. Randevu Geldi veya Tamamlandı durumundayken oluşturulabilir. Bekliyor veya Onaylandı randevuda rapor başlatılırsa randevu otomatik olarak **Geldi** durumuna geçer (K38); ek dokunuş istenmez.
- İçerik: önce fotoğrafı, sonra fotoğrafı, yapılandırılmış alanlar ve not. Hepsi isteğe bağlıdır; en az biri dolu olmalıdır.
- **Yapılandırılmış alanlar**, her biri tek seçimli chip:
  - Davranış: Sakin / Huzursuz
  - Cilt: Normal / Kuru veya tahriş / Yara veya kızarıklık
  - Kulak: Temiz / Kirli / Kızarık veya kokulu
  - Tüy: İyi / Keçeli / Dökülme fazla
  - Sonraki bakım önerisi: 3 / 4 / 6 / 8 hafta
- Aynı hayvanda "Huzursuz" son iki raporda işaretlenmişse "Huysuz / gergin" uyarı etiketi (HAY-02) eklemesi önerilir; owner onaylar.
- Fotoğraf kameradan veya galeriden eklenir.
- **Kolaj (K33):** önce ve sonra fotoğraflarının ikisi de varsa "Kolajla paylaş" seçeneği çıkar. Kolaj telefonda, paylaşım anında üretilir ve saklanmaz. Tek şablon: 4:5 dikey, üstte "Önce", altta "Sonra", köşede küçük harflerle salon adı. Fotoğraflardan biri eksikse seçenek görünmez, tek fotoğraf paylaşılır.
- "Gönder" butonu telefonun paylaşım menüsünü açar; bir görsel (kolaj ya da tek fotoğraf) ve hazır mesaj metni birlikte paylaşılır. Mesaj metni salon adını, hayvan adını, yapılandırılmış alanların özetini ve notu içerir.
- Müşterinin ileri tarihli sonuçlanmamış randevusu yoksa mesajın sonuna rebook cümlesi eklenir: "… için N hafta sonra yer ayıralım mı?" N, sonraki bakım önerisidir; boşsa 4.
- **Paylaşım hayvan başınadır.** Randevuda birden fazla hayvan varsa owner raporları sırayla paylaşır; her hayvan kartında kendi "Gönder" butonu vardır. Tek paylaşımda çoklu görsel yoktur.
- Paylaşım menüsü başarıyla tamamlandığında rapor "Paylaşıldı" olarak işaretlenir. Bu, mesajın WhatsApp'ta gerçekten gönderildiğini değil, paylaşımın başlatıldığını gösterir.
- Rapor paylaşıldıktan sonra da düzenlenebilir ve tekrar paylaşılabilir.
- Rapor fotoğrafları hayvanın fotoğraf geçmişine eklenir (HAY-04).
- Rapor tamamlamadan bağımsızdır (K40).

---

## Bitiş

**OPR-05 · Tamamlama** · Mobil + Web
*Salon sahibi olarak randevuyu tamamladığımda tahsilat ve sonraki randevu adımlarına sırayla yönlendirilmek istiyorum.*
- "Tamamla" butonu Bekliyor, Onaylandı ve Geldi durumlarında kullanılabilir (K38).
- Tamamlamadan önce onay istenir: "Tamamlanan randevu geri alınamaz."
- Tamamlandıktan sonra sırasıyla şu adımlar gelir:
  1. **Tahsilat** (E7): hizmet dökümü ve ödeme alma
  2. **Sonraki randevu** önerisi (OPR-06)
- Her adım atlanabilir. Tahsilat atlanırsa tutar müşterinin borcuna yazılır (E7).
- Tamamlanan randevu müşterinin "son ziyaret" bilgisini ve "Yeni" etiketini günceller (MUS-01).
- Tamamlanan randevu geri alınamaz (K37).

**OPR-06 · Sonraki randevu** · Mobil + Web
*Salon sahibi olarak tamamlanan randevudan, aynı hayvanlar ve hizmetlerle dolu bir sonraki randevu formu açabilmek istiyorum.*
- "3 / 4 / 6 / 8 hafta sonra" seçenekleri sunulur.
- Varsayılan seçenek, müşterinin son iki tamamlanmış randevusu arasındaki süreye en yakın seçenektir. Yeterli geçmiş yoksa varsayılan 4 haftadır.
- Seçim yapılınca, seçilen haftanın aynı günü ve aynı saatiyle, aynı hayvanlar ve hizmetlerle dolu bir randevu formu açılır.
  - Fiyatlar K32 kuralıyla güncel değerlerden gelir, önceki randevudan kopyalanmaz.
  - Arşivlenmiş hayvanlar forma gelmez.
  - Kaydetme RAN-02 ve RAN-04 kurallarıyla yapılır.
- Müşteri o an randevu almak istemezse **"Şimdi değil"** seçilir; seçili aralık kadar sonrası için müşteriye bir **rebook hatırlatma tarihi** kaydedilir (K39, E6).
- Müşterinin ileri tarihli sonuçlanmamış bir randevusu oluşursa, rebook hatırlatma tarihi geçersiz olur.
- Sonraki randevu adımı, tamamlanmış bir randevunun detayından sonradan da açılabilir.

---

## Kapsam dışı

- Tamamlanan randevuyu geri alma (K37)
- Check-in için müşteri imzası veya QR kod
- "Hayvan hazır, gelip alabilirsiniz" bildirimi (E6'da şablon olarak; butonu OPR-02 hayvan kartına E6 ekler)
- Bakım sırasında süre ölçümü (time tracking)
- Hayvan bazında rebook hatırlatması
- Bakım raporunun web sayfası olarak paylaşılması (yalnızca paylaşım menüsü)
- Müşteriye canlı durum linki (Sırada → Geldi → Banyoda → Hazır), Faz 2

## Teknik notlar

| Konu | Karar | Story |
| --- | --- | --- |
| Durum geçişleri | İzin verilen geçişler sunucuda tanımlıdır; tabloda olmayan geçiş API'de `409 INVALID_STATUS_TRANSITION` ile reddedilir. Enum: `pending \| confirmed \| arrived \| completed \| no_show \| cancelled`. | OPR-01 |
| Durum renkleri | Story'deki hex değerler tasarım referansıdır; kodda token kullanılır (`global.css` / `tailwind.config.js`, `lib/theme.ts`). | OPR-01 |
| "Geldi mi?" sorusu | Saklanmaz; `endAt < şimdi` ve durum Bekliyor/Onaylandı ise istemci gösterir. Owner cevap vermezse randevu Bekliyor kalır ve soru görünmeye devam eder. | OPR-01 |
| Gelmedi sayısı | Saklanmaz; müşterinin "Gelmedi" durumundaki randevu sayısından hesaplanır. | OPR-01, MUS-04 |
| Ek ücret | `appointmentLine` tablosunda `type: service \| extra`. Ek ücret satırında `extraChargeId`, kopyalanmış ad ve tutar bulunur; `serviceId` boş, `durationMin` 0'dır. E4 veri modeli notu buna göre güncellendi. | OPR-03 |
| Bakım raporu | `groomingReport` (`appointmentPetId`, `beforePhotoId`, `afterPhotoId`, `behavior`, `skinFinding`, `earFinding`, `coatFinding`, `nextCareWeeks`, `note`, `sharedAt`). Enum'lar API'de tanımlı, hepsi nullable. Fotoğraflar HAY-04 ile aynı depolamayı ve multipart yüklemeyi kullanır. | OPR-04 |
| Rapor başlatma | Rapor oluşturma isteği, randevu `pending` veya `confirmed` ise sunucuda aynı transaction'da `arrived`'a geçirir. Durum geçiş tablosuna örtük geçiş olarak eklidir. | OPR-04, OPR-01 |
| Paylaşım | Hayvan başına tek görsel + metin. `expo-sharing` yalnızca dosya paylaştığı için metin React Native `Share` ile verilir; Android'de görsel ve metin ayrı ayrı paylaşılabilir, cihaz testine göre karar. Yeni kütüphane yok. | OPR-04 |
| Kolaj | İstemcide görünüm yakalama: `react-native-view-shot`, [RULES.md](../rules.md) kütüphane tablosuna "görsel birleştirme" satırı olarak eklenir. Çıktı geçici dosyadır, paylaşımdan sonra silinir. | OPR-04 |
| Son ziyaret | OPR-02 yanıtında hayvan başına `lastReport` (nullable): `note`, `afterPhotoUrl`, `behavior`; ayrıca `referencePhotoUrl`. Ayrı istek yok. | OPR-02 |
| Etiket önerisi | İstemci, son iki raporun `behavior` değerinden türetir; saklanmaz. | OPR-04 |
| Tamamlama | `completedAt` saklanır. Tamamlama isteği döküm oluşturma (E7) ile aynı işlemde, tek transaction'da yapılır. | OPR-05 |
| Rebook tarihi | Müşteride `rebookReminderAt`. İleri tarihli sonuçlanmamış randevu varken hatırlatma sorguları bu müşteriyi dışarıda bırakır. | OPR-06 |
| Kod yeri | Randevu detayı, durum geçişleri ve rapor `features/appointments/` altında; sonraki randevu formu RAN-02 bileşenini yeniden kullanır. | Tümü |

## Diğer epic'lere bağlantılar

| Buradan | Oraya | Konu |
| --- | --- | --- |
| OPR-01 | MUS-04 | Gelmedi sayısı |
| OPR-01 | RAN-01, RAN-02, RAN-06 | "Geldi mi?" takvim bloğunda, geçmiş randevu, iptal durumu |
| OPR-01 | E6 | Onay linki `confirmed` üretir |
| OPR-02 | HAY-02, HAY-03, RAN-04, RAN-07 | Uyarı şeritleri ve not |
| OPR-02 | INT-02, INT-03 | Formu gönder, "Bakım onayı yok" işareti |
| OPR-02 | HAY-04 | Son ziyaret fotoğrafı ve referans fotoğrafı |
| OPR-04 | HAY-02 | "Huzursuz" tekrarında etiket önerisi |
| OPR-03 | KUR-08 | Ek ücret kalemleri |
| OPR-04 | HAY-04 | Rapor fotoğraflarının fotoğraf geçmişine eklenmesi |
| OPR-05 | E7 | Döküm, tahsilat, borç |
| OPR-05 | MUS-01 | Son ziyaret ve "Yeni" etiketi |
| OPR-06 | RAN-02, RAN-04 | Sonraki randevunun kaydedilmesi |
| OPR-06 | E6 | Rebook hatırlatması |

## Açık sorular

- Yok.
