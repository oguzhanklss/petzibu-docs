# E2 · Müşteri & Hayvan

| Durum     | Faz | Platform    | Bağımlılık                                          |
| --------- | --- | ----------- | --------------------------------------------------- |
| Yapılacak | MVP | Mobil + Web | [E1](01-kurulum-isletme.md) (boyut kademesi tanımı) |

> Story'ler: Kesinleşti v1.4 · 23 Eylül 2026

## Amaç

Müşteri ve hayvan hakkındaki her bilgi tek yerde dursun, saniyeler içinde bulunsun.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [README](index.md#alınan-kararlar).

| #   | Karar                                                                                                                           |
| --- | ------------------------------------------------------------------------------------------------------------------------------- |
| K1  | Bir randevu içinde birden fazla hayvan olabilir. Bir müşterinin birden fazla hayvanı olabilir.                                  |
| K5  | Tüm müşteri ve hayvan kayıtları bir işletmeye aittir.                                                                           |
| K12 | Boyut kademesi kilodan önerilir. Sabit eşikler: küçük < 10 kg, orta 10–25 kg, büyük > 25 kg.                                    |
| K16 | Müşteri başına **tek telefon** vardır ve işletme içinde benzersizdir. İkinci telefon alanı yoktur.                              |
| K17 | Hayvan uyarı etiketleri **sabit bir listeden** seçilir. Müşteri etiketleri serbest metindir.                                    |
| K18 | Aşı için son yapılma tarihi girilir. Geçerlilik bitişi otomatik olarak **son tarih + 12 ay** hesaplanır ve elle düzeltilebilir. |
| K19 | Hayvan türleri yalnızca **Köpek** ve **Kedi**.                                                                                  |
| K20 | Hayvan silinmez, **arşivlenir**. Kalıcı silme yalnızca müşteri silme ile olur (E8).                                             |
| K32 | Satır fiyatı: hayvan + hizmet özel fiyatı varsa o, yoksa kademe fiyatı. Son fiyat yalnızca bilgi.                               |
| K35 | Müşteri ve hayvan verisi için veri sorumlusu salon, Petzibu veri işleyen.                                                       |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

| ID     | Başlık                              | Platform    | Durum     |
| ------ | ----------------------------------- | ----------- | --------- |
| MUS-01 | Müşteri listesi ve arama            | Mobil + Web | Yapılacak |
| MUS-02 | Müşteri ekleme                      | Mobil + Web | Yapılacak |
| MUS-03 | Rehberden içe aktarma               | Mobil       | Yapılacak |
| MUS-04 | Müşteri detayı                      | Mobil + Web | Yapılacak |
| MUS-05 | Müşteri düzenleme, etiketler ve not | Mobil + Web | Yapılacak |
| MUS-06 | Hızlı iletişim                      | Mobil       | Yapılacak |
| HAY-01 | Hayvan ekleme ve düzenleme          | Mobil + Web | Yapılacak |
| HAY-02 | Uyarı etiketleri                    | Mobil + Web | Yapılacak |
| HAY-03 | Aşı takibi                          | Mobil + Web | Yapılacak |
| HAY-04 | Fotoğraf geçmişi                    | Mobil       | Yapılacak |
| HAY-05 | Hayvanı arşivleme                   | Mobil + Web | Yapılacak |

---

## Müşteri

**MUS-01 · Müşteri listesi ve arama** · Mobil + Web
_Salon sahibi olarak müşterilerimi isim, telefon veya hayvan adıyla arayabilmek istiyorum, böylece "Paşa'nın sahibi kimdi?" sorusunu saniyede çözerim._

- Liste satırında şunlar görünür:
  - Müşteri adı
  - Hayvan adları (arşivlenenler hariç)
  - Son ziyaret ("3 hafta önce")
  - "Yeni" etiketi (tamamlanmış randevusu yoksa)
  - Son ziyaret ve "Yeni", randevu verisinden gelir. E4 hazır olana kadar API bu alanları boş döner ve satır onları gizler.
- Mobilde liste alfabetik sıralanır ve harf başlıklarıyla gruplanır (tasarım 8).
- Arama tek bir kutudan yapılır ve müşteri adı, telefon ve hayvan adında arar.
- Telefon numarası boşluklu, boşluksuz, başında 0 ile ya da +90 ile yazılsa da bulunur.
- Arama Türkçe karakterlere ve büyük/küçük harfe duyarsızdır: "sukru" yazınca "Şükrü" bulunur.
- Filtre chip'leri: Tümü / Köpek / Kedi / Borçlu. "Borçlu" filtresi E7 hazır olunca görünür.
- Web'de liste tablo olarak gösterilir: ad, hayvanlar, telefon, toplam ödeme, randevu sayısı, yaklaşan randevu. Kolonlar sıralanabilir.

**MUS-02 · Müşteri ekleme** · Mobil + Web
_Salon sahibi olarak yeni müşteriyi yalnızca ad ve telefonla, 10 saniyede ekleyebilmek istiyorum._

- Zorunlu alanlar: ad soyad (tek alan) ve telefon.
- Telefon Türkiye formatında doğrulanır ve "+90" ön ekiyle saklanır.
- Telefon işletme içinde benzersizdir (K16). Aynı numara girilirse "Bu numara Ayşe Yılmaz'a kayıtlı" mesajı ve o müşteriye git seçeneği gösterilir; kayıt oluşturulmaz.
- İsteğe bağlı **"Eski borç"** alanı: girilirse müşteri için randevusuz bir açılış dökümü oluşur (KAS-01). E7 hazır olana kadar alan gizlidir.
- Müşteri eklendikten sonra "Hayvan ekle" adımı önerilir, ama atlanabilir.
- Randevu formundan da müşteri eklenebilir (RAN-02). Orada telefon alanına yazılmış numara, yeni müşteri formuna dolu gelir.

**MUS-03 · Rehberden içe aktarma** · Mobil
_Salon sahibi olarak telefon rehberimdeki müşterileri seçip aktarabilmek istiyorum, böylece ilk gün sıfırdan başlamam._

- Rehber izni yalnızca bu ekran açıldığında istenir.
- Kişiler tek tek seçilir. **"Tümünü seç" seçeneği yoktur ve hiçbir kişi varsayılan olarak seçili gelmez** (Apple 5.1.2(v), KVKK).
- Listede arama yapılabilir.
- Petzibu'da zaten kayıtlı olan numaralar listede işaretli ve seçilemez halde görünür.
- Birden fazla numarası olan kişide mobil numara önerilir; owner başka bir numarayı seçebilir.
- Numarası Türkiye formatına çevrilemeyen kişiler seçilemez ve nedeni gösterilir.
- Aktarım sonunda "12 müşteri eklendi" özeti gösterilir.
- Rehber izni verilmezse ekran bunu açıklar ve müşteri elle eklenebilir.

**MUS-04 · Müşteri detayı** · Mobil + Web
_Salon sahibi olarak müşteriyi açtığımda onu tek ekranda tanımak istiyorum: ne kadar değerli, ne kadar riskli, hangi hayvanları var._

- Üst blok: ad, telefon, etiketler, ara ve WhatsApp butonları (MUS-06).
- Müşteri bakım riski onayını (INT-03) hiç vermemişse üst blokta küçük bir "Bakım onayı yok" işareti görünür; dokununca "Formu gönder" (INT-02) açılır. Telefonla eklenen müşteriler bu işaretle başlar.
- Özet metrikler:
  - Yaklaşan
  - Tamamlanan
  - İptal (müşteri tarafından yapılanlar)
  - **Gelmedi**
  - **Borç**
  - Toplam ödeme
- Gelmedi sayısı ve borç sıfırdan büyükse vurgulu gösterilir.
- Her metriğe dokununca ilgili randevu listesi açılır.
- Borç ve toplam ödeme metrikleri E7'deki tahsilat modeline bağlıdır; E7 hazır olana kadar gizli kalır.
- Hayvanlar bölümü: yatay kartlar ve sonda "+ Hayvan ekle" kartı. Aşı sorunu veya uyarı etiketi olan hayvanın kartında ikon görünür. Arşivlenen hayvanlar ayrı bir "Arşiv" bölümünde, kapalı halde durur.
- Randevular bölümü: önce yaklaşanlar, sonra geçmiştekiler (tarih, hayvanlar, hizmetler, tutar, durum).
- Not alanı (MUS-05) bu ekranda görünür.

**MUS-05 · Müşteri düzenleme, etiketler ve not** · Mobil + Web
_Salon sahibi olarak müşteri bilgilerini düzenleyebilmek, "VIP", "Pazarlık yapar" gibi etiketler ve serbest not ekleyebilmek istiyorum._

- Ad ve telefon düzenlenebilir. Telefon değişirken de benzersizlik kuralı geçerlidir.
- Açılış dökümü yoksa "Eski borç" buradan da girilebilir; varsa düzenleme döküm üzerinden yapılır (KAS-02).
- Etiketler serbest metinle girilir. İşletmede daha önce kullanılmış etiketler öneri olarak gösterilir.
- Etiketler müşteri listesinde ve randevu detayındaki müşteri kartında (OPR-02) görünür.
- Not alanı tek ve serbest metindir; müşteriye ait genel bilgiler içindir (ör. "Öğleden sonra aranmayı tercih ediyor").

**MUS-06 · Hızlı iletişim** · Mobil
_Salon sahibi olarak müşteri kartından tek dokunuşla arama yapabilmek veya WhatsApp'ı açabilmek istiyorum._

- "Ara" butonu telefonun arama ekranını numara dolu olarak açar.
- "WhatsApp" butonu bu numarayla boş bir sohbet açar.
- Bu iki buton müşteri detayında, randevu detayında ve müşteri listesinde (satırı sola kaydırınca) bulunur.
- WhatsApp yüklü değilse web üzerinden WhatsApp açılır.

---

## Hayvan

**HAY-01 · Hayvan ekleme ve düzenleme** · Mobil + Web
_Salon sahibi olarak bir müşteriye birden fazla hayvan ekleyebilmek ve bilgilerini düzenleyebilmek istiyorum._

- Zorunlu alanlar: ad ve tür (Köpek / Kedi, K19).
- Diğer alanlar: fotoğraf, ırk, kilo, doğum yılı, **boyut kademesi**, huy notu, alerji/sağlık notu, tıraş tercihi.
- Boyut kademesi kilodan önerilir (K12):
  - Kilo ilk kez girildiğinde kademe otomatik seçilir.
  - Kilo sonradan değişir ve önerilen kademe mevcut kademeden farklı olursa "Boyutu Büyük yapalım mı?" önerisi gösterilir. Kademe otomatik değişmez.
  - Kilo girilmemişse kademe elle seçilir.
  - Kademe seçilmemişse randevu oluştururken sorulur (RAN-02).
- Irk alanı serbest metindir; seçilen türe göre yaygın ırklardan otomatik tamamlama önerisi gelir.
- **Özel fiyatlar** (K32): hayvan profilinde hayvan + hizmet bazında sabitlenmiş fiyatlar listelenir (ör. "Tıraş · 500 ₺"). Buradan kaldırılabilir. Yeni özel fiyat randevu formundan "Paşa için bu fiyatı sabitle" ile eklenir (RAN-02); profilden doğrudan eklenmez.
- Yaş, doğum yılından hesaplanıp gösterilir ("4 yaş").
- Uyarı etiketleri (HAY-02) ve aşı bilgileri (HAY-03) aynı formda düzenlenir (tasarım 17).

**HAY-02 · Uyarı etiketleri** · Mobil + Web
_Salon sahibi olarak "Isırır", "Ağızlık" gibi uyarıları hayvana etiketleyebilmek istiyorum, böylece bu uyarılar takvimde ve randevu detayında her zaman görünür._

- Etiketler sabit bir listeden, ikonlarıyla seçilir (K17):
  - Isırır
  - Ağızlık gerekli
  - Huysuz / gergin
  - Hassas cilt
  - Alerjik
  - Yaşlı / hassas
  - Kalp / sağlık sorunu
  - Makas, makine veya kurutmadan korkar
- Bir hayvana birden fazla etiket eklenebilir.
- Etiketler takvim bloğunda ikon olarak (RAN-01), randevu detayında uyarı şeridi olarak (OPR-02) gösterilir.
- Etiketin detaylı açıklaması huy ve sağlık notu alanlarına yazılır.

**HAY-03 · Aşı takibi** · Mobil + Web
_Salon sahibi olarak kuduz ve karma aşılarının tarihlerini girebilmek ve süresi geçmek üzere olan veya geçmiş aşılar için uyarılmak istiyorum._

- Takip edilen aşılar: kuduz ve karma.
- Her aşı için son yapılma tarihi girilir. Geçerlilik bitişi otomatik olarak son tarih + 12 ay hesaplanır ve elle düzeltilebilir (K18).
- Durumlar:
  - Geçerli (teal #2A9D90)
  - 30 gün içinde dolacak (amber #E8C468)
  - Süresi geçmiş (kırmızı #EF4444)
  - Bilinmiyor (gri), tarih girilmemişse
- Durum her gün güncel tarihe göre hesaplanır.
- Aşı durumu hayvan profilinde, randevu detayında ve randevu formunda gösterilir (tasarım 16, 18, 22).
- Aşı sorunu hiçbir durumda randevuyu engellemez; yalnızca uyarı verir.
- Aşı süresi dolma hatırlatması E6'da ele alınır.

**HAY-04 · Fotoğraf geçmişi** · Mobil
_Salon sahibi olarak her bakımın fotoğraflarının hayvanın profilinde tarihli olarak birikmesini istiyorum, böylece müşteri "geçen seferki gibi" dediğinde fotoğrafa bakarak yaparım._

- Bakım raporundaki önce/sonra fotoğrafları (OPR-04) otomatik olarak geçmişe eklenir.
- Galeriden veya kameradan elle de fotoğraf eklenebilir.
- Fotoğraflar en yeniden en eskiye sıralanır ve tarih etiketiyle gösterilir. Dokununca tam ekran açılır.
- Tek bir fotoğraf silinebilir.
- Bir fotoğraf **referans** olarak işaretlenebilir ("Böyle kesilsin"). Hayvan başına en fazla bir referans; profilde sabit görünür ve randevu detayındaki hayvan kartında gösterilir (E5).
- Fotoğraflar yüklenmeden önce sıkıştırılır.
- Web'de fotoğraflar görüntülenebilir; fotoğraf ekleme yalnızca mobilde.

**HAY-05 · Hayvanı arşivleme** · Mobil + Web
_Salon sahibi olarak artık gelmeyen veya vefat eden bir hayvanı arşivleyebilmek istiyorum, böylece müşteriye yanlış bir hatırlatma gitmez ama geçmiş kayıtları kaybolmaz._

- Arşivlenen hayvan için yeni randevu oluşturulamaz ve hiçbir hatırlatma (rebook, aşı) gönderilmez.
- Hayvanın ileri tarihli, henüz başlamamış (Bekliyor veya Onaylandı) randevusu varsa **arşivleme yapılamaz**. Owner'a bu randevuların listesi gösterilir; her satıra dokununca ilgili randevu açılır ve owner orada hayvanı randevudan çıkarır (RAN-05) ya da randevuyu iptal eder (RAN-06). Liste boşalınca "Arşivle" butonu aktif olur.
- Bu akış yeni bir iş mantığı içermez: randevu düzenleme ve iptal E4'teki akışlarla yapılır. Süre ve fiyatın yeniden hesaplanması ve iptalin kime yazılacağı orada çözülür.
- Hayvan müşteri detayında "Arşiv" bölümüne taşınır. Geçmiş randevuları ve fotoğrafları korunur.
- Arşivden geri alınabilir.
- Arayüzde "vefat" kelimesi kullanılmaz; eylemin adı "Arşivle"dir.
- Hayvan kalıcı olarak silinemez (K20).

---

## Kapsam dışı

- İkinci telefon veya ikinci iletişim kişisi
- Tekrarlanan müşteri kayıtlarını birleştirme (telefonun benzersiz olması çoğunu önler)
- Hayvanı başka bir müşteriye taşıma
- Köpek ve kedi dışındaki türler
- Kuduz ve karma dışındaki aşılar
- Serbest (işletmeye özel) uyarı etiketleri
- Excel'den toplu içe aktarma (Faz 2, web)
- Müşteri silme ve anonimleştirme (E8)

## Teknik notlar

| Konu               | Karar                                                                                                                                                                                                            | Story                  |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| Müşteri adı        | Tek alan: `name` (ad soyad). Ayrı ad/soyad yok.                                                                                                                                                                  | MUS-02, MUS-03, MUS-05 |
| Telefon formatı    | Tüm telefonlar E.164 formatında (`+905551234567`) saklanır. Benzersizlik bu normalize değer üzerinden kontrol edilir.                                                                                            | MUS-02, MUS-03, MUS-05 |
| Arama              | Arama; ad, hayvan adı ve normalize telefon üzerinde yapılır. Türkçe karakterler aramadan önce sadeleştirilir (ş→s, ı→i, ğ→g vb.).                                                                                | MUS-01                 |
| Alfabetik sıralama | Harf gruplama ve sıralama Türkçe alfabeye göre yapılır (Ç, Ş, İ yerli yerinde). `localeCompare(..., 'tr')`.                                                                                                      | MUS-01                 |
| Aşı verisi         | Aşı kaydında `lastDate` ve `validUntil` saklanır. `validUntil` varsayılan olarak `lastDate + 12 ay` ile doldurulur. Durum saklanmaz, `validUntil` ile bugünün tarihinden hesaplanır.                             | HAY-03                 |
| Aşı renkleri       | Story'deki hex değerler tasarım referansıdır. Kodda token kullanılır (`global.css` / `tailwind.config.js`); yeni token gerekiyorsa orada tanımlanır.                                                             | HAY-03                 |
| Uyarı etiketleri   | Sabit liste istemcide ve API'de enum olarak tanımlanır.                                                                                                                                                          | HAY-02                 |
| Boyut kademesi     | `sizeTier` hayvanda nullable. Kademe boşken randevu formu sorar (RAN-02).                                                                                                                                        | HAY-01                 |
| Özel fiyat         | `petServicePrice` (`petId`, `serviceId`, `price`); hayvan + hizmet başına en fazla bir kayıt. Hizmet pasife alınınca kayıt kalır, görünmez.                                                                      | HAY-01                 |
| Referans fotoğrafı | Fotoğrafta `isReference` bayrağı; hayvan başına en fazla bir true.                                                                                                                                               | HAY-04                 |
| Arşiv              | Hayvanda `archivedAt` alanı. Randevu oluşturma ve hatırlatma sorguları arşivlenmiş hayvanları dışarıda bırakır.                                                                                                  | HAY-05                 |
| Rehber             | Rehbere yalnızca MUS-03 ekranında erişilir; izin reddedilirse uygulama akışı bozulmaz. `expo-contacts` eklenir; Expo Go'da çalışır.                                                                              | MUS-03                 |
| Hızlı iletişim     | `tel:` ve `whatsapp://send?phone=` şemaları; WhatsApp yoksa `https://wa.me/`. Yeni kütüphane gerekmez.                                                                                                           | MUS-06                 |
| Fotoğraf           | Yüklemeden önce istemcide sıkıştırılır (`expo-image-manipulator`). Yükleme kendi API'ye **multipart** ile yapılır; `lib/api.ts`'e multipart desteği eklenir, backend dosyayı alıp depolar. Presigned URL yok.    | HAY-04, HAY-01         |
| Liste alanları     | `lastVisitAt` (nullable) ve `isNew` müşteri listesi yanıtında yer alır. E4 öncesi `lastVisitAt` boş, `isNew` true gelir. Alan adları E4 teknik notlarıyla aynıdır.                                               | MUS-01                 |
| Arşiv kilidi       | Arşivleme isteği, ileri tarihli Bekliyor/Onaylandı randevu varsa `409 PET_HAS_UPCOMING_APPOINTMENTS` ile reddedilir ve yanıt randevu listesini taşır. İstemci de aynı kontrolü butonu pasifleştirmek için yapar. | HAY-05                 |

## Diğer epic'lere bağlantılar

| Buradan        | Oraya          | Konu                                                  |
| -------------- | -------------- | ----------------------------------------------------- |
| HAY-01         | KUR-07, RAN-02 | Boyut kademesi ve otomatik fiyat                      |
| HAY-02, HAY-03 | RAN-01, OPR-02 | Takvimde ikon, randevu detayında uyarı şeridi         |
| HAY-03         | E6             | Aşı süresi dolma hatırlatması                         |
| HAY-04         | OPR-04         | Bakım raporu fotoğrafları                             |
| HAY-05         | E6, RAN-02     | Arşivlenen hayvana hatırlatma ve randevu yok          |
| HAY-05         | RAN-05, RAN-06 | Arşiv öncesi randevudan çıkarma veya iptal            |
| MUS-04         | E7             | Borç ve toplam ödeme metrikleri                       |
| MUS-02, MUS-05 | KAS-01 | "Eski borç" alanı açılış dökümü oluşturur |
| MUS-02         | RAN-02         | Randevu formundan müşteri ekleme                      |
| —              | E3             | Intake formu aynı müşteri ve hayvan modelini doldurur |
| —              | E8             | Müşteri silme ve anonimleştirme                       |

## Açık sorular

- Yok. ("Makastan ürker" sorusu, mevcut "Kurutma makinesinden korkar" etiketinin "Makas, makine veya kurutmadan korkar" olarak genişletilmesiyle kapandı; liste 8 etikette kaldı.)
