# E3 · Intake Form

| Durum     | Faz | Platform                                                                            | Bağımlılık                                                                                                                                                             |
| --------- | --- | ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Yapılacak | MVP | Form müşteri tarafında web'de açılır; gönderme, inceleme ve onay tarafı Mobil + Web | [E2](02-musteri-hayvan.md) (müşteri ve hayvan modeli), [E8](08-hesap-veri.md) (KVKK metni), [E5](05-randevu-operasyonu.md) (randevu detayındaki "Formu gönder" butonu) |

> Story'ler: Kesinleşti v1.4 · 23 Eylül 2026

## Amaç

Müşteri kendi kaydını ve hayvanlarının kaydını kendisi yapsın. Owner'a veri girişi işi çıkmasın.

## Çözdüğü sorun

Groomer telefonda müşteriyle konuşurken bir yandan hayvanın ırkını, kilosunu, huyunu, aşılarını not etmeye çalışıyor. Intake form bu yükü müşteriye aktarır:

1. Müşteri arar. Groomer randevu formunda yalnızca ad, telefon ve hayvanın adını ve türünü girer (10 saniye).
2. Groomer "Formu gönder"e basar; WhatsApp hazır mesajla açılır.
3. Müşteri formu dilediği zaman doldurur: ırk, kilo, huy, aşılar, fotoğraf.
4. Groomer tek dokunuşla onaylar; bilgiler **mevcut müşteri ve hayvan kaydına** eklenir.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [README](index.md#alınan-kararlar).

| # | Karar |
| --- | --- |
| K3 | Gönderim yarı otomatiktir: uygulama hazır WhatsApp mesajını açar, owner gönderir. |
| K14 | Form, MVP'deki küçük public web yüzeyinde (davet ve şifre sıfırlama sayfalarıyla aynı yerde) çalışır. |
| K21 | Form Petzibu'nun kendi formudur. Google Forms ürünün parçası değildir; yalnızca ürün öncesi araştırma aracı olarak kullanılabilir. |
| K22 | İki link türü vardır: işletme başına **genel link** ve müşteriye bağlı **kişiye özel link**. İkisi aynı mekanizmayla çalışır. |
| K23 | Her form, kaynağı ne olursa olsun, owner **onayından geçmeden** kayıt oluşturmaz veya güncellemez. |
| K24 | Form **sabit şablondur**. Alan açma/kapama ayarı yoktur (K6'nın yerine geçer). Zorunlu alanlar yalnızca: sahip ad soyad, telefon, hayvan adı, hayvan türü. |
| K25 | Aşı karnesi fotoğrafı istenmez; yalnızca aşı tarihleri sorulur. Rakip analizi v2 sonrası yeniden teyit edildi; keşif görüşmelerinde ihtiyaç çıkarsa açılır. |
| K34 | İşletme Askıda iken intake linki kapalıdır. |
| K35 | Müşteri ve hayvan verisi için veri sorumlusu salon, Petzibu veri işleyen. Formdaki aydınlatma metni işletme bazlıdır. |
| K46 | Push bildirimi MVP'de yok; yeni form yalnızca rozetle gösterilir. |
| K48 | İşletme adresi zorunludur; KVKK metnindeki adres her zaman doludur. |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

| ID     | Başlık                          | Platform                                | Durum     |
| ------ | ------------------------------- | --------------------------------------- | --------- |
| INT-01 | Genel form linki                | Mobil + Web                             | Yapılacak |
| INT-02 | Kişiye özel form linki gönderme | Mobil + Web                             | Yapılacak |
| INT-03 | Müşterinin formu doldurması     | Public web (müşteri tarafı)             | Yapılacak |
| INT-04 | Başvuru kutusu ve bildirim      | Mobil + Web                             | Yapılacak |
| INT-05 | İnceleme ve onay                | Mobil + Web                             | Yapılacak |
| INT-06 | Formu reddetme                  | Mobil + Web                             | Yapılacak |

INT-03 mobil uygulamada iş çıkarmaz; public web yüzeyinde (K14) ve backend'de yapılır. Burada duruyor çünkü INT-04, INT-05 ve INT-06 onun ürettiği veriye bağımlı.

---

## Owner tarafı: link

**INT-01 · Genel form linki** · Mobil + Web
_Salon sahibi olarak salonuma ait sabit bir form linkine sahip olmak istiyorum, böylece onu Instagram biyografimde veya WhatsApp durumumda paylaşabilirim._

- Her işletmenin tek bir genel linki vardır; işletme oluşturulurken otomatik üretilir (ADM-01).
- Link ayarlardan kopyalanabilir ve telefonun paylaşım menüsüyle paylaşılabilir.
- Link yenilenebilir. Yenilenince eski link çalışmaz; eski linki açan kişiye "Bu link artık geçerli değil" mesajı gösterilir.
- Genel linkten gelen form hiçbir müşteriye önceden bağlı değildir; eşleştirme telefonla yapılır (INT-05).
**INT-02 · Kişiye özel form linki gönderme** · Mobil + Web
_Salon sahibi olarak telefonda konuştuğum müşteriye kişiye özel bir form linki gönderebilmek istiyorum, böylece müşteri bilgileri kendisi tamamlar ve bu bilgiler doğrudan o müşterinin kaydına eklenir._

- "Formu gönder" butonu şu yerlerde bulunur:
  - Müşteri detayı (MUS-04)
  - Randevu detayı (OPR-02)
  - Yeni müşteri kaydedildikten sonra çıkan ekran (MUS-02)
- Butona basınca WhatsApp, müşterinin numarasıyla ve hazır bir mesajla açılır. Mesajda salon adı ve link yer alır. Web'de buton `wa.me` linkini açar (MUS-06 ile aynı mekanizma).
- Link tek bir müşteriye bağlıdır ve süreli çalışır.
- Link tek gönderimliktir. Form gönderildikten sonra link tekrar açılırsa "Bu form zaten gönderildi" mesajı gösterilir.
- Aynı müşteri için yeni link oluşturulursa önceki kullanılmamış link geçersiz olur.
- Kişiye özel linkte müşterinin adı, telefonu ve kayıtlı (arşivlenmemiş) hayvanlarının adı ve türü formda dolu gelir. Telefon alanı değiştirilemez.

---

## Müşteri tarafı: form

**INT-03 · Müşterinin formu doldurması** · Public web (müşteri tarafı)
_Pet sahibi olarak üyelik açmadan, telefonumdan birkaç dakikada salonun formunu doldurmak istiyorum._

- Tek sayfalık, mobil öncelikli, Türkçe bir form. Üstte salon adı yazar.
- Bölümler:
  - **Sahip:** ad soyad, telefon.
  - **Hayvan:** ad, tür (Köpek / Kedi), ırk, yaklaşık kilo, doğum yılı, fotoğraf.
  - **Huy ve sağlık:** HAY-02'deki sabit uyarı listesi, müşteriye uygun dille onay kutuları olarak sorulur (ör. "Yabancılara karşı ısırma eğilimi var", "Makas, makine veya kurutmadan korkuyor"). Ayrıca serbest alerji / sağlık notu.
  - **Aşılar:** kuduz ve karma için son yapılma tarihi. Her biri için "Bilmiyorum" seçeneği vardır.
  - **Tıraş tercihi:** serbest metin.
- "+ Başka bir hayvan ekle" ile birden fazla hayvan girilebilir; eklenen hayvan kaldırılabilir.
- Zorunlu alanlar yalnızca: sahip ad soyad, telefon, hayvan adı, hayvan türü (K24). Diğer her şey isteğe bağlıdır.
- Telefon Türkiye formatında doğrulanır.
- KVKK aydınlatma metni linkle gösterilir ve onay kutusu işaretlenmeden form gönderilemez. Metin **işletme bazlıdır**: veri sorumlusu olarak salonun adı ve adresi (KUR-05; adres zorunlu, K48) metne otomatik dolar; Petzibu veri işleyen olarak geçer (K35). Şablon E8'de tanımlanır.
- **Bakım riski onayı:** sabit bir onay kutusu (keçe açma, kısa tıraş, yaşlı veya hasta hayvan riskleri). İşaretlenmeden form gönderilemez. Onay zamanı ve metin sürümü KVKK onayıyla aynı mekanizmayla saklanır (INT-05, E8).
- İşletme Askıda ise (K34) form açılmaz; müşteri "Bu işletme şu an form kabul etmiyor" mesajını görür. Genel ve kişiye özel link için aynı kural.
- Fotoğraf yüklemesi başarısız olursa form kaybolmaz; kullanıcı tekrar deneyebilir veya fotoğrafsız gönderebilir.
- Gönderim sonrası teşekkür ekranı gösterilir; salon adı ve salon telefonu bu ekranda yer alır.
- Genel link spam'e karşı korunur: istek sayısı sınırlanır ve görünmez bir tuzak alan (honeypot) kullanılır.

---

## Owner tarafı: inceleme

**INT-04 · Başvuru kutusu ve bildirim** · Mobil + Web
_Salon sahibi olarak yeni bir form geldiğinde haberdar olmak ve bekleyen formları tek bir yerde görmek istiyorum._

- Bekleyen formlar tek bir listede görünür; sayısı sekme rozetiyle gösterilir. Rozet uygulama açılınca ve liste yenilenince güncellenir.
- Push bildirimi yoktur (K46); yeni form yalnızca rozetle görünür.
- Her satırda şunlar bulunur:
  - Sahibin adı
  - Hayvan adları
  - Geliş zamanı
  - Eşleşme etiketi: "Yeni müşteri" veya "Mevcut: Ayşe Yılmaz"
- Liste en yeni form en üstte olacak şekilde sıralanır.
- İncelenmeyen form **30 gün** sonra fotoğraflarıyla birlikte otomatik silinir (KVKK). Silinmeye 7 gün kala satırda ve rozette uyarı görünür ("3 form 7 gün içinde silinecek").
**INT-05 · İnceleme ve onay** · Mobil + Web
_Salon sahibi olarak gelen formu inceleyip tek dokunuşla onaylamak istiyorum, böylece kayıtlar kendiliğinden oluşur._

- Eşleştirme kuralları:
  - Kişiye özel linkten gelen form, linkin bağlı olduğu müşteriye eşleşir.
  - Genel linkten gelen form, normalize telefon numarasına göre mevcut müşteriyle eşleşir. Eşleşme yoksa yeni müşteri oluşturulur.
  - Hayvanlar, aynı müşteri içinde ada göre eşleşir. Büyük/küçük harf ve Türkçe karakter farkı gözetilmez. Arşivlenmiş hayvanlar eşleşmeye dahil değildir.
  - Aynı adla birden fazla hayvan eşleşirse onay ekranı o hayvan için "Hangisi?" diye sorar; seçenekler mevcut hayvanlar ve "Yeni hayvan olarak ekle". Varsayılan, yeni hayvan olarak eklemektir.
- Birleştirme kuralları:
  - Form, mevcut kayıttaki **dolu alanların üzerine yazmaz**; yalnızca boş alanları doldurur ve yeni hayvanları ekler.
  - Tek istisna aşı tarihleridir: formdaki son aşı tarihi mevcut tarihten daha yeniyse güncellenir ve geçerlilik bitişi yeniden hesaplanır (K18).
  - Formda işaretlenen uyarı etiketleri, hayvanın mevcut etiketlerine eklenir; mevcut etiketler kaldırılmaz.
  - Kilo girilmişse ve hayvanın boyut kademesi boşsa, kademe kilodan belirlenir (K12).
- Onay ekranı, onaylanınca **nelerin oluşacağını ve nelerin güncelleneceğini** gösterir.
- Formdaki değeri mevcut kayıttan farklı olan alanlar ayrıca "Farklı bilgiler" başlığı altında listelenir. Bu değerler otomatik uygulanmaz; owner isterse kaydı elle düzeltir.
- Onaylanan formdaki fotoğraflar hayvanın fotoğraf geçmişine (HAY-04) ve hayvanın fotoğrafı boşsa profil fotoğrafı olarak eklenir.
- KVKK onayının zamanı ve onaylanan metnin sürümü müşteri kaydında saklanır.
- Onaydan sonra form kutudan çıkar ve owner oluşan veya güncellenen müşteri kaydına yönlendirilir.
**INT-06 · Formu reddetme** · Mobil + Web
_Salon sahibi olarak spam veya hatalı bir formu kayıt oluşturmadan silebilmek istiyorum._

- Reddetmeden önce onay istenir.
- Reddedilen formun verisi, fotoğrafları dahil, kalıcı olarak silinir. İşlenmeyecek kişisel veri saklanmaz (KVKK).
- Kişiye özel linkten gelen form reddedilirse, owner aynı müşteriye yeni bir link gönderebilir.

---

## Kapsam dışı

- Form builder ve alan açma/kapama ayarı
- İmzalı sözleşme ve e-imza (INT-03'teki bakım riski onay kutusu bunun dışındadır; kutu var, imza yok)
- Aşı karnesi fotoğrafı
- Formdan randevu talebi veya online rezervasyon (Faz 3)
- Onaysız otomatik kayıt
- Formun Türkçe dışındaki dillerde gösterilmesi
- Pazarlama izni (İYS) toplama (Faz 3, pazarlama ile birlikte)
- Push bildirimi (K46)

## Ürün öncesi araştırma (öneri)

Kod yazılmadan önce, pilot salonda iki hafta boyunca bir Google Form ile aynı akış denenebilir: groomer telefonda arayan müşterilere formu WhatsApp'tan gönderir. Fotoğraf sorusu konmaz (Google Forms'ta dosya yükleme, müşterinin Google hesabıyla giriş yapmasını gerektirir). Öğrenilmek istenenler:

- Müşterilerin kaçı formu dolduruyor?
- Hangi sorular boş bırakılıyor?
- Form ortalama kaç dakikada tamamlanıyor?
Sonuçlar INT-03'teki alan listesini kesinleştirmek için kullanılır.

## Teknik notlar

| Konu | Karar | Story |
| ----------------- | --- | ------------------------- |
| Link modeli | Tek tablo: `intakeLink` (`businessId`, `customerId` nullable, `token`, `expiresAt` nullable, `usedAt`, `revokedAt`). `customerId` boşsa genel link, doluysa kişiye özel link. Genel linkin süresi yoktur. | INT-01, INT-02 |
| Link süresi | Kişiye özel link 14 gün geçerlidir. | INT-02 |
| Başvuru verisi | Gönderilen form, onaya kadar `intakeSubmission` kaydında ham veri (JSON) olarak durur; müşteri ve hayvan tablolarına onaya kadar dokunulmaz. | INT-03, INT-05 |
| Fotoğraflar | Form fotoğrafları geçici bir alana yüklenir; onayda hayvanın fotoğraf geçmişine taşınır, redde silinir. Yükleme yöntemi HAY-04 ile aynıdır (multipart, object storage). | INT-03, INT-05, INT-06 |
| Eşleştirme | Telefon E.164 formatında karşılaştırılır. Hayvan adı, E2'deki arama sadeleştirmesiyle (Türkçe karakter ve büyük/küçük harf) karşılaştırılır. | INT-05 |
| Uyarı etiketleri | Formdaki onay kutuları, HAY-02'deki enum ile birebir eşleşir; yalnızca müşteriye gösterilen metin farklıdır. | INT-03 |
| KVKK | Onay zamanı ve aydınlatma metni sürümü saklanır. Metnin sürümlenmesi E8'de tanımlanır. Bakım riski onayı da aynı yapıyla (`consentType`, `version`, `acceptedAt`) tutulur. | INT-03, INT-05 |
| Spam | Genel link için IP başına istek sınırı ve honeypot alan. | INT-03 |
| Saklama süresi | `intakeSubmission.createdAt` + 30 gün; ADM-02'deki günlük job siler. Liste yanıtında `expiresAt` döner, istemci 7 gün kala uyarır. | INT-04 |
| Bildirim | Push yok (K46); yalnızca rozet. Bekleyen form sayısı müşteri listesi / ayarlar sorgularından bağımsız küçük bir `GET /intake/submissions/count` ile alınır. | INT-04 |
| Public web yüzeyi | Backend (NestJS) sunucu tarafında render eder: davet kabulü, şifre sıfırlama, intake formu. Ayrı web repo yok. INT-03 backend projesinde takip edilir. Owner web back office (K47) bundan ayrıdır. | INT-03 |
| Belirsiz eşleşme | `preview` yanıtı her form hayvanı için `matchedPetId`, `candidates[]` döner; birden fazla aday varsa istemci seçim ister ve onay isteğinde `petId` ya da `null` (yeni) gönderir. | INT-05 |
| Onay işlemi | Onay tek bir API çağrısıdır ve atomiktir: müşteri, hayvanlar, etiketler, aşılar ve fotoğraflar tek transaction'da yazılır. İstemci "nelerin oluşacağı" önizlemesini ayrı bir `preview` uç noktasından alır; birleştirme mantığı yalnızca backend'de yaşar. | INT-05 |
| Mobil kod yeri | Link üretme, kutu, önizleme, onay ve red `features/intake/` altında. INT-03 bu repoda değildir. | INT-01, INT-02, INT-04…06 |

## Diğer epic'lere bağlantılar

| Buradan        | Oraya                  | Konu                                                           |
| -------------- | ---------------------- | -------------------------------------------------------------- |
| INT-01         | ADM-01                 | Genel link işletme oluşturulurken üretilir                     |
| INT-02         | MUS-02, MUS-04, OPR-02 | "Formu gönder" butonunun yerleri                               |
| INT-02         | MUS-06                 | Web'de `wa.me` mekanizması                                     |
| INT-02         | RAN-02                 | Randevu formunda hayvanın yalnızca ad ve türle hızlı eklenmesi |
| INT-03         | HAY-02, HAY-03         | Uyarı listesi ve aşı alanları                                  |
| INT-03         | KUR-05                 | Salon adı, adresi ve telefonu                                  |
| INT-05         | E2 (tümü)              | Müşteri ve hayvan modeline yazma, eşleştirme kuralları         |
| INT-05         | HAY-04                 | Form fotoğraflarının fotoğraf geçmişine eklenmesi              |
| INT-03         | K14                    | Public web yüzeyi                                              |
| INT-03, INT-05 | E8                     | KVKK aydınlatma metni ve sürümü                                |

## Açık sorular

- Yok.
