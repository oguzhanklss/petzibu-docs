# E5 · Randevu Operasyonu

| Durum     | Faz | Platform        | Bağımlılık                 |
| --------- | --- | --------------- | -------------------------- |
| Yapılacak | MVP | Mobil öncelikli | [E4](04-takvim-randevu.md) |

> Story'ler: Kesinleşti

## Amaç

Müşterinin salona gelişinden bakımın bitişine kadar olan süreç akıcı olsun.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [Epic Haritası](index.md#alınan-kararlar).

| #                               | Başlık                                                                                                           |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| [K1](index.md#alınan-kararlar)  | Bir randevu içinde birden fazla hayvan olabilir.                                                                 |
| [K2](index.md#alınan-kararlar)  | "Fatura", müşteriye gönderilen hizmet dökümü ve tahsilat kaydıdır.                                               |
| [K3](index.md#alınan-kararlar)  | Hatırlatmalar MVP'de yarı otomatik: uygulama mesajları kuyrukta listeler, owner tek dokunuşla WhatsApp'ı açar.   |
| [K26](index.md#alınan-kararlar) | Geçmiş tarihe randevu girilebilir.                                                                               |
| [K32](index.md#alınan-kararlar) | Randevu satırı fiyatı tek kuralla gelir: özel fiyat varsa o, yoksa kademe fiyatı.                                |
| [K33](index.md#alınan-kararlar) | Önce/sonra kolajı telefonda, paylaşım anında üretilir; saklanmaz.                                                |
| [K37](index.md#alınan-kararlar) | Tamamlanan randevu geri alınamaz.                                                                                |
| [K38](index.md#alınan-kararlar) | "Geldi" adımı atlanabilir.                                                                                       |
| [K39](index.md#alınan-kararlar) | Rebook hatırlatması müşteri bazındadır, hayvan bazında değil.                                                    |
| [K40](index.md#alınan-kararlar) | Bakım raporu tamamlama akışından bağımsızdır; zorunlu adım değildir.                                             |
| [K49](index.md#alınan-kararlar) | Rebook süresi tek kuralla belirlenir: son rapordaki öneri, yoksa son iki randevu arasındaki süre, yoksa 4 hafta. |
| [K52](index.md#alınan-kararlar) | Tek tıkla onay linki MVP'de.                                                                                     |
| [K60](index.md#alınan-kararlar) | Bakım raporu silinemez, üzerine yazılır.                                                                         |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

| ID     | Başlık                                    | Platform    | Durum     |
| ------ | ----------------------------------------- | ----------- | --------- |
| OPR-01 | Durum akışı                               | Mobil + Web | Yapılacak |
| OPR-02 | Randevu detayı                            | Mobil + Web | Yapılacak |
| OPR-03 | İşlem sırasında hizmet ve ek ücret ekleme | Mobil       | Yapılacak |
| OPR-04 | Bakım raporu                              | Mobil       | Yapılacak |
| OPR-05 | Tamamlama                                 | Mobil + Web | Yapılacak |
| OPR-06 | Sonraki randevu                           | Mobil + Web | Yapılacak |

---

## Durum ve detay

**OPR-01 · Durum akışı** · Mobil + Web
_Salon sahibi olarak randevunun hangi aşamada olduğunu tek dokunuşla güncelleyebilmek istiyorum._

- Durumlar ve renkleri:
  - Bekliyor (gri)
  - Onaylandı (gri + tik)
  - Geldi (amber #E8C468)
  - Tamamlandı (teal #2A9D90)
  - İptal (gri + üstü çizili; E4, RAN-06)
  - Gelmedi (kırmızı #EF4444)
- İzin verilen geçişler:

  | Durumdan   | Geçebileceği durumlar                        |
  | ---------- | -------------------------------------------- |
  | Bekliyor   | Onaylandı, Geldi, Tamamlandı, Gelmedi, İptal |
  | Onaylandı  | Geldi, Tamamlandı, Gelmedi, İptal            |
  | Geldi      | Tamamlandı, İptal                            |
  | Gelmedi    | Bekliyor (geri alma)                         |
  | Tamamlandı | — (K37)                                      |
  | İptal      | — (RAN-06)                                   |

- "Onaylandı" durumuna iki yoldan geçilir: owner elle işaretler (müşteri hatırlatmaya olumlu cevap verdiğinde) ya da müşteri tek tıkla onay linkine tıklar (HAT-05, K52). İkisi de aynı `confirmed` geçişidir.
- "Geldi" adımı atlanabilir (K38).
- Randevunun bitiş saati geçmiş ve durumu hâlâ Bekliyor veya Onaylandı ise, takvim bloğunda ve randevu detayında **"Geldi mi?"** sorusu gösterilir: "Tamamlandı" / "Gelmedi". Bu soru geçmiş tarihli yeni randevular için de gösterilir (RAN-02).
- "Gelmedi" olarak işaretlenen randevu müşterinin gelmedi sayısına (MUS-04) yansır ve takvimde görünür kalır (K27).
- "Gelmedi" geri alınırsa randevu Bekliyor durumuna döner ve gelmedi sayısından düşer.
- "Geldi" durumuna geçerken ek bir bilgi istenmez.
  **OPR-02 · Randevu detayı** · Mobil + Web
  _Salon sahibi olarak randevuyu açtığımda, işe başlamadan bilmem gereken her şeyi tek ekranda görmek istiyorum._
- **Üst bölüm:** durum chip'i, tarih ve saat aralığı, duruma göre değişen ana ve ikincil aksiyon butonları. Tabloda olmayan hiçbir geçiş ekranda görünmez (OPR-01):

  | Durum      | Ana aksiyon                  | İkincil aksiyonlar                       |
  | ---------- | ---------------------------- | ---------------------------------------- |
  | Bekliyor   | "Geldi"                      | "Tamamla" (OPR-05), "Onaylandı" işaretle |
  | Onaylandı  | "Geldi"                      | "Tamamla" (OPR-05)                       |
  | Geldi      | "Tamamla" (OPR-05)           | —                                        |
  | Tamamlandı | "Dökümü gör" (E7)            | "Sonraki randevu" (OPR-06)               |
  | Gelmedi    | "Geri al" (Bekliyor'a döner) | —                                        |
  | İptal      | —                            | —                                        |

- Bitiş saati geçmiş Bekliyor/Onaylandı randevuda ana aksiyonun yerini "Geldi mi?" sorusu alır: "Tamamlandı" / "Gelmedi" (OPR-01).
- Bekliyor/Onaylandı randevuda hatırlatma durumu görünür: "Hatırlatma gönderilmedi" / "Dün 18:05 gönderildi" (HAT-04).
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
  - Geldi durumunda "Hazır" butonu (HAT-08)
- **Ziyarete özel not** (RAN-07).
- **Alt bölüm:** toplam tutar.
- **"⋯" menüsü:** düzenle (RAN-05), iptal (RAN-06), "Gelmedi olarak işaretle" (yalnızca Bekliyor/Onaylandı), formu gönder (INT-02). Sonuçlanmış randevuda düzenle, iptal ve gelmedi görünmez.

---

## İşlem sırasında

**OPR-03 · İşlem sırasında hizmet ve ek ücret ekleme** · Mobil
_Salon sahibi olarak işe başladıktan sonra bir hayvana "keçe açma" gibi bir ek ücret veya ek hizmet ekleyebilmek istiyorum, böylece eklediğim kalem dökümde otomatik görünür._

- Randevu Bekliyor, Onaylandı veya Geldi durumundayken yapılabilir.
- Ek ücret, KUR-08'de tanımlı kalemlerden seçilir; tutarı o an değiştirilebilir.
- Ek ücret hayvan bazında eklenir ("Paşa: Keçe açma 150 ₺").
- Ek ücret randevunun süresini değiştirmez. Ek hizmet (RAN-05 ile aynı kural) süreyi ve bitiş saatini değiştirir; fiyatı K32 kuralıyla gelir.
- Ek ücretin adı ve tutarı randevuya o anki değerleriyle kopyalanır.
- Eklenen kalem kaldırılabilir.
- Web'de bu ekran yoktur; tamamlanmış randevuya web'den kalem eklemek KAS-02 ile yapılır. Tamamlanmamış randevu için web'de ek ücret eklenmez; bu masa başı işi değildir.
  **OPR-04 · Bakım raporu** · Mobil
  _Salon sahibi olarak her hayvan için önce/sonra fotoğraflı bir rapor oluşturup WhatsApp'tan göndermek istiyorum._
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
- "Gönder" butonu telefonun paylaşım menüsünü açar; bir görsel (kolaj ya da tek fotoğraf) ve hazır mesaj metni birlikte paylaşılır. Mesaj metni "Bakım raporu" şablonundan (HAT-02) üretilir: salon adı, hayvan adı, yapılandırılmış alanların özeti ve not.
- Müşterinin ileri tarihli sonuçlanmamış randevusu yoksa mesajın sonuna rebook cümlesi eklenir: "… için N hafta sonra yer ayıralım mı?" N, K49 kuralıyla belirlenir; bu raporda öneri seçildiyse önce o gelir.
- **Paylaşım hayvan başınadır.** Randevuda birden fazla hayvan varsa owner raporları sırayla paylaşır; her hayvan kartında kendi "Gönder" butonu vardır. Tek paylaşımda çoklu görsel yoktur.
- Paylaşım menüsü başarıyla tamamlandığında rapor "Paylaşıldı" olarak işaretlenir. Bu, mesajın WhatsApp'ta gerçekten gönderildiğini değil, paylaşımın başlatıldığını gösterir.
- Rapor paylaşıldıktan sonra da düzenlenebilir ve tekrar paylaşılabilir.
- Rapor fotoğrafları hayvanın fotoğraf geçmişine eklenir (HAY-04).
- Rapor tamamlamadan bağımsızdır (K40).

---

## Bitiş

**OPR-05 · Tamamlama** · Mobil + Web
_Salon sahibi olarak randevuyu tamamladığımda tahsilat ve sonraki randevu adımlarına sırayla yönlendirilmek istiyorum._

- "Tamamla" butonu Bekliyor, Onaylandı ve Geldi durumlarında kullanılabilir (K38). Yeri OPR-02'deki aksiyon tablosundadır.
- Tamamlamadan önce onay istenir: "Tamamlanan randevu geri alınamaz."
- Tamamlandıktan sonra sırasıyla şu adımlar gelir:
  1. **Tahsilat** (E7): hizmet dökümü ve ödeme alma
  2. **Sonraki randevu** önerisi (OPR-06)
- Her adım atlanabilir. Tahsilat atlanırsa tutar müşterinin borcuna yazılır (E7).
- Tamamlanan randevu müşterinin "son ziyaret" bilgisini ve "Yeni" etiketini günceller (MUS-01); müşteri gelmeyenler listesinden gizlenmişse gizleme kalkar (HAT-07).
- Tamamlanan randevu geri alınamaz (K37).
  **OPR-06 · Sonraki randevu** · Mobil + Web
  _Salon sahibi olarak tamamlanan randevudan, aynı hayvanlar ve hizmetlerle dolu bir sonraki randevu formu açabilmek istiyorum._
- "3 / 4 / 6 / 8 hafta sonra" seçenekleri sunulur.
- Varsayılan seçenek K49 kuralıyla gelir: bu randevudaki hayvanların raporlarında sonraki bakım önerisi varsa o (birden fazla hayvanda farklı öneriler varsa en kısası); yoksa müşterinin son iki tamamlanmış randevusu arasındaki süreye en yakın seçenek; yoksa 4 hafta.
- Seçim yapılınca, seçilen haftanın aynı günü ve aynı saatiyle, aynı hayvanlar ve hizmetlerle dolu bir randevu formu açılır.
  - Fiyatlar K32 kuralıyla güncel değerlerden gelir, önceki randevudan kopyalanmaz.
  - Arşivlenmiş hayvanlar forma gelmez.
  - Kaydetme RAN-02 ve RAN-04 kurallarıyla yapılır.
- Müşteri o an randevu almak istemezse **"Şimdi değil"** seçilir; seçili aralık kadar sonrası için müşteriye bir **rebook hatırlatma tarihi** kaydedilir (K39, HAT-06).
- Müşterinin ileri tarihli sonuçlanmamış bir randevusu oluşursa, rebook hatırlatma tarihi geçersiz olur. Müşterinin arşivlenmemiş hayvanı kalmazsa da temizlenir (HAY-05).
- Sonraki randevu adımı, tamamlanmış bir randevunun detayından sonradan da açılabilir (OPR-02 ikincil aksiyon).

---

## Kapsam dışı

- Tamamlanan randevuyu geri alma (K37)
- Check-in için müşteri imzası veya QR kod
- Bakım sırasında süre ölçümü (time tracking)
- Hayvan bazında rebook hatırlatması
- Bakım raporunun web sayfası olarak paylaşılması (yalnızca paylaşım menüsü)
- **Bakım raporunu kaldırma** (K60). Rapor hayvan başına tekildir ve yazma upsert'tir, yani yanlış içerik üzerine yazılarak düzeltilir; "bu hayvana hiç rapor olmayacaktı" durumu OPR-04'te bir ihtiyaç olarak tanımlı değil. İhtiyaç çıkarsa kendi story'si olur ve fotoğraf kayıtlarının ne olacağı (rapor silinince `PetPhoto` hayvanın geçmişinde kalır mı) o zaman karara girer.
- Web'de tamamlanmamış randevuya ek ücret ekleme (KAS-02 tamamlanmış randevuyu kapsar)
- Müşteriye canlı durum linki (Sırada → Geldi → Banyoda → Hazır), Faz 2

## Teknik notlar

| Konu               | Karar                                                                                                                                                                                                                                                                                                                                   | Story          |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| Durum geçişleri    | İzin verilen geçişler sunucuda tek bir durum servisinde tanımlıdır; tabloda olmayan geçiş API'de `409 INVALID_STATUS_TRANSITION` ile reddedilir. Enum: `pending \| confirmed \| arrived \| completed \| no_show \| cancelled`. HAT-05 onay sayfası da aynı servisi çağırır. Sonuçlanmış / sonuçlanmamış tanımı E4 teknik notlarındadır. | OPR-01         |
| Durum renkleri     | Story'deki hex değerler tasarım referansıdır; kodda token kullanılır (`global.css` / `tailwind.config.js`, `lib/theme.ts`).                                                                                                                                                                                                             | OPR-01         |
| "Geldi mi?" sorusu | Saklanmaz; `endAt < şimdi` ve durum Bekliyor/Onaylandı ise istemci gösterir. Owner cevap vermezse randevu Bekliyor kalır ve soru görünmeye devam eder.                                                                                                                                                                                  | OPR-01         |
| Risk sayaçları     | Gelmedi ve yaklaşan randevu sayısı **saklanmaz**, sayılır. OPR-02 detayı bunun için yalın bir sayma metodu kullanır (iki sorgu); müşteri listesinin metodu genişletilmez (listede yaklaşan sayısı gösterilmiyor, MUS-01) ve müşteri detayının geçmiş listesi de tekrar yüklenmez — S5 sonrası detay gövdesi her aksiyon dokunuşunda dönüyor ve bedeli her seferinde ödenir. Ayrışmayı önleyen şey sorgunun tekliği değil **kuralın** tekliği: "yaklaşan" = sonuçlanmamış ve `startAt >= şimdi`, "gelmedi" = `status = no_show`; iki tanım modül düzeyinde tek yerde durur ve bütün çağıranlar onu kullanır. MUS-04'teki sayaçlarla aynı değeri verir.                                                                                                                                                                                                                                                              | OPR-01, MUS-04 |
| Ek ücret           | `appointmentLine` tablosunda `type: service \| extra`. Ek ücret satırında `extraChargeId`, kopyalanmış ad ve tutar bulunur; `serviceId` boş, `durationMin` 0'dır. E4 veri modeli notu buna göre güncellendi. **Satır hayvana bağlanır:** OPR-03 ek ücreti hayvan başına ekler ("Paşa: Keçe açma"), yani `appointmentPetId` dolu olur; boş `appointmentPetId` yalnızca salon düzeyindeki kalemdir (perakende). Sözleşmede ikisi ayrı taşınır: hayvanın altında `extras`, randevunun altında `extraLines`. Satır **katalogdan seçilir**: yazma gövdesi `extraChargeId`'yi zorunlu tutar, serbest giriş OPR-03'te yoktur (`price` yalnızca o anki tutar düzeltmesidir). Yanıttaki `extraChargeId` yine nullable kalır ama boş hâlinin kapısı OPR-03 değil **KAS-02**'dir: döküm satırları ayrı bir kopya değil, tamamlanmış randevunun `appointmentLine` kayıtlarıdır (E7) ve katalogsuz satırı açacaksa oradan açılır. Kalem tek tek kaldırılabildiği için ek ücret satırı sözleşmede `id` taşır (E4'te taşımıyordu) ve `PUT /appointments/:id` ek ücretlere **dokunmaz** — ekleme ve kaldırma kendi uçlarındadır, yoksa düzenleme formu eklenen kalemi sessizce silerdi.                                                                                                                            | OPR-03         |
| Bakım raporu       | `groomingReport` (`appointmentPetId`, `beforePhotoId`, `afterPhotoId`, `behavior`, `skinFinding`, `earFinding`, `coatFinding`, `nextCareWeeks`, `note`, `sharedAt`). Enum'lar API'de tanımlı, hepsi nullable. Fotoğraflar HAY-04 ile aynı depolamayı ve multipart yüklemeyi kullanır.                                                   | OPR-04         |
| Rapor başlatma     | Rapor oluşturma isteği, randevu `pending` veya `confirmed` ise sunucuda aynı transaction'da `arrived`'a geçirir. Durum geçiş tablosuna örtük geçiş olarak eklidir.                                                                                                                                                                      | OPR-04, OPR-01 |
| Rapor mesajı       | Mesaj metnini backend üretir (HAT-02 şablon servisi); OPR-02 yanıtındaki hayvan kartı `reportMessage` alanını taşır. İstemci şablon çözmez. Rebook cümlesinin koşulu olan "müşterinin ileri tarihli sonuçlanmamış randevusu" **bu randevuyu hariç tutar**: rapor `arrived` randevuya yazılıyor, `arrived` sonuçlanmamış bir durum ve `pending` randevuya rapor yazılınca örtük geçişle `arrived` oluyor — başlangıcı hâlâ ileri tarihte olabilir. Dışlama olmazsa koşulu sağlayan şey randevunun kendisi olur ve cümle tam gerektiği yerde düşer. RAN-04 çakışma motoru aynı tuzağı aynı yolla çözüyor (dışlanacak randevu id'si parametredir). HAT-02'deki şablon servisi E6'da yazılana kadar metin **koda gömülü varsayılan şablondan** üretilir ve `MessageTemplate` tablosuna dokunulmaz; şablon servisi gelince aynı alan oradan beslenir, sözleşme değişmez.                                                                                                                                                                                             | OPR-04         |
| Paylaşım           | Hayvan başına tek görsel + metin. `expo-sharing` yalnızca dosya paylaştığı için metin React Native `Share` ile verilir; Android'de görsel ve metin ayrı ayrı paylaşılabilir, cihaz testine göre karar. Yeni kütüphane yok.                                                                                                              | OPR-04         |
| Kolaj              | İstemcide görünüm yakalama: `react-native-view-shot`, [mobil RULES.md](../rules-mobile.md) kütüphane tablosuna "görsel birleştirme" satırı olarak eklenir. Çıktı geçici dosyadır, paylaşımdan sonra silinir.                                                                                                                            | OPR-04         |
| Hatırlatma durumu  | OPR-02 yanıtında tek alan: `lastReminderSentAt` (nullable), bu randevunun en yeni **gönderilmiş** hatırlatma satırının `sentAt`'ı (`ReminderLog`, HAT-04). İstemci boşken "Hatırlatma gönderilmedi", doluyken "Dün 18:05 gönderildi" yazar. Slot bilgisi (K50) detaya girmez — OPR-02 metni hangi zaman diliminden geldiğini söylemiyor, kuyruk ekranı HAT-03'ün işi. E5 bu alanı sözleşmeye koyar ve **her zaman boş** döner; E6 doldurur, sözleşme değişmez. | OPR-02 |
| Son ziyaret        | OPR-02 yanıtında hayvan başına `lastReport` (nullable): `at` (o randevunun `startAt`'ı), `note`, `afterPhotoUrl`, `behavior`; ayrıca `referencePhotoUrl`. Ayrı istek yok. **Raporsuz ziyaretler atlanır:** `lastReport`, bu randevu dışındaki tamamlanmış randevular arasında **raporu olan en yenisinin** raporudur. Rapor zorunlu olmadığı için (K40) yalnızca en son tamamlanan randevuya bakmak alanı sık sık boşaltır ve "son iki raporun `behavior`'ı"na dayanan etiket önerisini de sessizce durdurur. `at` bu yüzden gerekli: atlanan ziyaretlerden sonra gösterilen rapor iki yıl öncesinden gelebilir ve tarihsiz bir "Son ziyaret" bloğu onu geçen haftaymış gibi okutur.                                                                                                                                                                                                | OPR-02         |
| Detay gövdesi      | OPR-02 **ayrı bir şemayla** döner: `appointmentDetailResponse` = `appointmentResponse` + `lastReport`, rapor durumu, müşteri kartı ve risk sayaçları. Bu gövdeyi `GET /appointments/:id` **ve E5'in aksiyon uçları** (durum geçişi, ek ücret ekleme/kaldırma, rapor, tamamlama) döner; E4'ün `POST` ve `PUT /appointments`'ı hafif gövdede kalır, çünkü detay alanları geçmiş randevulara ek sorgu demektir ve **kaydetme** yolunda bedava değildir. Ayrım kaydetme ile aksiyon arasındadır, HTTP metodu arasında değil: kaydetme formu açıp kapatır ve detay alanlarına bakmaz, aksiyon ise zaten açık olan detay ekranında olur ve değiştirdiği şeyler (durum, `total`, `extras`, `suggestedRebookWeeks`) o gövdenin kendi alanlarıdır. Hafif gövde dönmek istemciyi her dokunuştan sonra ikinci bir `GET`'e zorlardı — aynı sorgular, bir tur fazla.                                                                                     | OPR-02         |
| Rapor okuma        | Raporun kendisi **detay gövdesinin içinde** taşınır: hayvan kartı `report: groomingReportResponse \| null` alır (bu randevunun kendi raporu; `sharedAt`, ön imzalı fotoğraf adresleri ve `reportMessage` dahil). Ayrı `GET .../report` ucu **yoktur** ve "rapor durumu" ayrı bir alan değildir: istemci `report`'tan türetir (`null` → rapor yok, `sharedAt` boş → hazır, dolu → paylaşıldı). `lastReport` bundan ayrıdır ve **önceki** ziyaretin özetidir. Rapor uçları da detay gövdesini döner (bkz. Detay gövdesi); `groomingReportResponse` yalnızca gömülü kullanılır, tek başına bir ucun yanıtı olmaz. Gerekçe: detay ekranı açıkken rapor formunun ön dolgusu ve paylaşım metni elde olmalı, yoksa istemci ya ikinci bir uca ya şablonu kendisi çözmeye zorlanırdı. | OPR-02, OPR-04 |
| Etiket önerisi     | İstemci, son iki raporun `behavior` değerinden türetir; saklanmaz.                                                                                                                                                                                                                                                                      | OPR-04         |
| Rebook süresi      | Sunucu hesaplar ve `completed` randevunun detay gövdesinde `suggestedRebookWeeks` döner (K49: rapor `nextCareWeeks` → son iki tamamlanmış randevu arası → 4). **K49'un ilk basamağının kapsamı iki yerde farklıdır, fallback'i ortaktır.** Randevu düzeyi (`suggestedRebookWeeks`): bu randevudaki hayvanların `nextCareWeeks` değerlerinin **en kısası**. Hayvan düzeyi (`reportMessage`'ın rebook cümlesi): **yalnızca o hayvanın** raporundaki değer; yoksa doğrudan fallback'e düşer, diğer hayvanın önerisine **değil** — bir hayvanın bakım aralığını başka hayvanın raporundan söylemek yanlış bilgi olurdu. Basamak 2 ve 3 tek bir metottur, iki çağıran da onu kullanır. Basamak 2'nin üç sınırı: **randevunun kendisi "son iki"ye sayılır** (aksi hâlde tam bir önceki ziyareti olan müşteri basamak 2'ye hiç giremez ve herkes 4'e düşerdi); aralık `startAt` ile ölçülür, `completedAt` ile değil (bakım döngüsü tıraş gününe göre işler ve geçmiş tarihli randevu (K26) bugün tamamlanınca `completedAt` aralığı uydurur); aralık gün cinsinden {21, 28, 42, 56} güne göre en yakına yuvarlanır ve **eşitlikte kısa olan kazanır** (5 hafta → 4), randevu düzeyindeki "en kısası" eğilimiyle aynı yön.                                                                                            | OPR-04, OPR-06 |
| Tamamlama          | `completedAt` saklanır. Tamamlama isteği döküm oluşturma (E7) ile aynı işlemde, tek transaction'da yapılır; `customer.hiddenFromInactiveAt` boşaltılır (HAT-07). **Dökümün kendisi E7'ye bırakıldı:** E5 tamamlaması `completedAt`, durum geçişi ve gizleme temizliğini yazar, `Statement` ve işletme bazlı numarası (KAS-01) E7'de aynı transaction'a eklenir — numaralandırma, indirim, ödeme ve PDF tek sahipte kalsın diye yarısı önceden yazılmaz.                                                                                                                                                                        | OPR-05         |
| Rebook tarihi      | Müşteride `rebookReminderAt`. **Temel randevunun `startAt`'ının yerel günüdür, `completedAt` değil:** OPR-06'nın form ön dolgusu ("seçilen haftanın aynı günü ve aynı saati") `startAt`'tan türüyor ve K49 aralığı da `startAt` ile ölçülüyor (bkz. Rebook süresi), yani iki temel kullanılırsa "Şimdi değil" ile kaydedilen tarih aynı seçimle açılacak formun tarihinden kayar. Geçmiş tarihli randevu (K26) bugün tamamlanınca fark haftalara çıkar; bakım döngüsü tıraş gününe göre işler. `completedAt` saklanır ve tamamlamanın anıdır, rebook hesabına girmez. İleri tarihli sonuçlanmamış randevu varken hatırlatma sorguları bu müşteriyi dışarıda bırakır. Arşivlenmemiş hayvan kalmayınca HAY-05 alanı boşaltır. Uç `POST /appointments/:id/rebook-later` (gövde `weeks`), yalnızca `completed` randevuda (`409 APPOINTMENT_NOT_COMPLETED`) ve **`204` döner**: yazdığı alan müşterinin, randevu gövdesinde karşılığı yok, detay gövdesi dönmek değişmeyen bir gövde için aynı sorguları yeniden koşturmak olurdu. | OPR-06         |
| Kod yeri           | Randevu detayı, durum geçişleri ve rapor `features/appointments/` altında; sonraki randevu formu RAN-02 bileşenini yeniden kullanır.                                                                                                                                                                                                    | Tümü           |

## Diğer epic'lere bağlantılar

| Buradan | Oraya                          | Konu                                                      |
| ------- | ------------------------------ | --------------------------------------------------------- |
| OPR-01  | MUS-04                         | Gelmedi sayısı                                            |
| OPR-01  | RAN-01, RAN-02, RAN-06         | "Geldi mi?" takvim bloğunda, geçmiş randevu, iptal durumu |
| OPR-01  | HAT-05                         | Onay linki `confirmed` üretir                             |
| OPR-02  | HAY-02, HAY-03, RAN-04, RAN-07 | Uyarı şeritleri ve not                                    |
| OPR-02  | INT-02, INT-03                 | Formu gönder, "Bakım onayı yok" işareti                   |
| OPR-02  | HAY-04                         | Son ziyaret fotoğrafı ve referans fotoğrafı               |
| OPR-02  | HAT-04, HAT-08                 | Hatırlatma durumu ve "Hazır" butonu                       |
| OPR-04  | HAY-02                         | "Huzursuz" tekrarında etiket önerisi                      |
| OPR-04  | HAT-02                         | Rapor mesajı şablonu                                      |
| OPR-03  | KUR-08, KAS-02                 | Ek ücret kalemleri; tamamlanmış randevuda düzeltme        |
| OPR-04  | HAY-04                         | Rapor fotoğraflarının fotoğraf geçmişine eklenmesi        |
| OPR-05  | E7                             | Döküm, tahsilat, borç                                     |
| OPR-05  | MUS-01, HAT-07                 | Son ziyaret, "Yeni" etiketi, gelmeyenler gizlemesi        |
| OPR-06  | RAN-02, RAN-04                 | Sonraki randevunun kaydedilmesi                           |
| OPR-06  | HAT-06                         | Rebook hatırlatması                                       |
| OPR-06  | HAY-05                         | Arşivlenmemiş hayvan kalmayınca rebook tarihi temizlenir  |

## Açık sorular

- Yok. (Web'de ek ücret sorusu KAS-02 ile kapandı: tamamlanmış randevu web'den dökümde düzeltilir, OPR-03 Mobil kalır.)
