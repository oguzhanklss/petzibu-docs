# E1 · Kurulum & İşletme

| Durum     | Faz | Platform        | Bağımlılık       |
| --------- | --- | --------------- | ---------------- |
| Yapılacak | MVP | Mobil öncelikli | Yok (temel epic) |

> Story'ler: Kesinleşti

## Amaç

Owner 5 dakikada uygulamayı kullanmaya hazır olsun.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [Epic Haritası](index.md#alınan-kararlar).

| # | Başlık |
| --- | --- |
| [K4](index.md#alınan-kararlar) | Hizmetlerde boyut kademesi var: küçük / orta / büyük için ayrı süre ve fiyat. |
| [K5](index.md#alınan-kararlar) | Tenant salon sahibi değil, işletmedir. |
| [K8](index.md#alınan-kararlar) | Giriş e-posta + şifre ile yapılır. |
| [K9](index.md#alınan-kararlar) | Uygulamada kayıt ekranı yok. |
| [K10](index.md#alınan-kararlar) | Ödeme uygulama dışında alınır. |
| [K11](index.md#alınan-kararlar) | Pilot → ücretli geçişte hesap ve veri aynı kalır; yalnızca işletmenin durumu değişir. |
| [K12](index.md#alınan-kararlar) | Boyut kademesi kilo eşikleri sabit: küçük < 10 kg, orta 10–25 kg, büyük > 25 kg. |
| [K13](index.md#alınan-kararlar) | Çalışma saatlerinde öğle arası yok. |
| [K14](index.md#alınan-kararlar) | Davet ve şifre sıfırlama linkleri her zaman web'de açılır. |
| [K15](index.md#alınan-kararlar) | Onboarding ilerlemesi adım numarası olarak saklanmaz. |
| [K34](index.md#alınan-kararlar) | Ödeme gecikince 7 gün Ödeme bekliyor, sonra Askıda. |
| [K35](index.md#alınan-kararlar) | KVKK rolleri: owner verisinde veri sorumlusu Petzibu; müşteri ve hayvan verisinde salon, Petzibu veri işleyen. |
| [K36](index.md#alınan-kararlar) | Hizmetin bir türü vardır: Köpek / Kedi / İkisi. |
| [K47](index.md#alınan-kararlar) | Owner web back office ayrı bir React uygulamasıdır. |
| [K48](index.md#alınan-kararlar) | İşletme adresi zorunludur. |
| [K57](index.md#alınan-kararlar) | Hesap silme şifreyle onaylanır; işletme Silinecek durumuna geçer, 30 gün içinde vazgeçilebilir. |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

| ID     | Başlık                                 | Platform          | Durum     |
| ------ | -------------------------------------- | ----------------- | --------- |
| ADM-01 | İşletme oluşturma ve owner davet etme  | İç araç (backend) | Yapılacak |
| ADM-02 | İşletme durumunu yönetme               | İç araç (backend) | Yapılacak |
| KUR-01 | Daveti kabul edip hesabı aktifleştirme | Mobil + Web       | Yapılacak |
| KUR-02 | Giriş ve oturum                        | Mobil + Web       | Yapılacak |
| KUR-03 | Şifremi unuttum                        | Mobil + Web       | Yapılacak |
| KUR-04 | Onboarding sihirbazı                   | Mobil             | Yapılacak |
| KUR-05 | İşletme bilgileri                      | Mobil + Web       | Yapılacak |
| KUR-06 | Çalışma saatleri                       | Mobil + Web       | Yapılacak |
| KUR-07 | Hizmet ekleme ve düzenleme             | Mobil + Web       | Yapılacak |
| KUR-08 | Ek ücret kalemleri                     | Mobil + Web       | Yapılacak |
| KUR-09 | Özel kapalı günler                     | Mobil + Web       | Yapılacak |

---

## İç araçlar (Petzibu ekibi)

ADM story'leri mobil uygulamada iş çıkarmaz; backend projesinde yapılır. Burada duruyorlar çünkü KUR-01 ve KUR-02 onlara bağımlı.

**ADM-01 · İşletme oluşturma ve owner davet etme** · İç araç
_Petzibu ekibi olarak yeni bir salon için işletme ve owner hesabı açıp owner'a davet e-postası göndermek istiyorum, böylece salon kendi başına kayıt olmadan sisteme girer._

- Girilen bilgiler: işletme adı, owner adı soyadı, owner e-postası.
- İşletme **Pilot** durumunda oluşturulur.
- Aynı e-postayla ikinci bir owner hesabı açılamaz.
- Owner'a tek kullanımlık, süreli bir davet linki gönderilir.
- Süresi dolan veya kaybolan davet yeniden gönderilebilir; eski link geçersiz olur.
- MVP'de bu bir yönetim paneli olmak zorunda değil; basit bir iç script veya sayfa yeterli.
**ADM-02 · İşletme durumunu yönetme** · İç araç
_Petzibu ekibi olarak bir işletmeyi pilot, aktif, ödeme bekliyor veya askıda durumuna alabilmek istiyorum, böylece ödemesi gelmeyen salonun erişimini veri kaybetmeden kısıtlayabilirim._

- Durumlar: **Pilot**, **Aktif**, **Ödeme bekliyor**, **Askıda** (K34), **Silinecek** (K57). İlk dördünü Petzibu ekibi buradan değiştirir; Silinecek'e yalnızca owner kendisi alır (HES-07) ve yalnızca owner vazgeçebilir.
- Durum değişikliği hesabı ve veriyi etkilemez. Pilottan aktife geçişte her şey olduğu gibi kalır (K11).
- **Ödeme bekliyor:** ödeme gecikince 7 gün ek süre. Bu sürede her şey çalışır; owner'a her gün uyarı gösterilir. Uyarı metni mobilde "Hesabınla ilgili bir konu var, Petzibu ile iletişime geç" biçimindedir; ödeme çağrısı, fiyat veya link içermez (K10). Web'de aynı uyarı "Ödeme bilgileri" bağlantısı taşır. 7 günün sonunda hesap Askıda olur.
- **Askıda ve Silinecek durumlarında çalışanlar:** kayıtları görüntüleme ve arama; müşteriyi arama ve WhatsApp butonları; veri dışa aktarma (HES-05); KVKK işlemleri (müşteri silme HES-04, hesap silme HES-07).
- **Askıda ve Silinecek durumlarında çalışmayanlar:** yeni kayıt ve düzenleme; hatırlatma kuyruğu (HAT-03); intake form linki (müşteri "Bu işletme şu an form kabul etmiyor" mesajını görür, INT-01/INT-03); onay linki (HAT-05).
- Askıdayken üstte "Hesabın askıda, verilerin güvende" şeridi görünür. Mobilde "Petzibu ile iletişime geç" butonu (WhatsApp veya e-posta açar); web'de "Ödeme bilgileri" butonu IBAN ve iletişim sayfasını açar. Mobilde ödeme linki, fiyat veya "abone ol" ifadesi yoktur (K10). Silinecek durumunda şerit metni ve "Vazgeç" butonu HES-07'de tanımlıdır.
- Ödeme MVP'de manueldir (havale veya ödeme linki); ödeme entegrasyonu yoktur. Petzibu ekibi ödemeyi ADM-02'den işaretleyince hesap Aktif olur ve kaldığı yerden devam eder.
- Kontrol backend'de tek yerdedir: askıdaki veya silinecek işletmenin yazma istekleri `403 TENANT_SUSPENDED` ile reddedilir, okuma istekleri çalışır. İstemci bu kodu `lib/api.ts` içinde tek yerde yakalar, şeridi gösterir ve yazma eylemlerini pasifleştirir.
- Askı durumu en fazla 12 ay sürer; sonunda veri silinir. Silmeden 30 gün önce owner'a haber verilir. Silinecek durumu 30 gün sürer (K57). İki durumun silme yolu aynıdır.

---

## Giriş ve oturum

**KUR-01 · Daveti kabul edip hesabı aktifleştirme** · Mobil + Web
_Salon sahibi olarak davet e-postasındaki linkle şifremi belirleyip uygulamaya girmek istiyorum._

- Link, hangi cihazda tıklanırsa tıklansın web sayfasında açılır (K14).
- Sayfada owner'ın adı ve e-postası dolu gelir; owner yalnızca şifre belirler (en az 8 karakter).
- KVKK aydınlatma metni, kullanım koşulları ve **veri işleme sözleşmesi** (salon veri sorumlusu, Petzibu veri işleyen; K35) onaylanmadan hesap aktifleşmez. Onaylanan metinlerin sürümü ve zamanı `consent` tablosuna yazılır (HES-03).
- Davet linkiyle giriş, e-posta adresini doğrulanmış sayar; ayrı bir doğrulama adımı yoktur.
- Link kullanılmış veya süresi dolmuşsa, owner'a Petzibu ekibiyle iletişime geçmesi söylenir.
- Şifre belirlenince başarı ekranı gösterilir: "Uygulamayı aç" butonu ve App Store / Google Play linkleri.
- Owner uygulamada ilk kez giriş yaptığında onboarding sihirbazı (KUR-04) başlar.
**KUR-02 · Giriş ve oturum** · Mobil + Web
_Salon sahibi olarak bir kez giriş yaptıktan sonra her seferinde şifre girmek istemiyorum._

- Giriş ekranında yalnızca e-posta, şifre ve "Şifremi unuttum" bulunur. "Kayıt ol" butonu yoktur (K9).
- Oturum cihazda açık kalır; kullanıcı çıkış yapana kadar tekrar şifre istenmez.
- Aynı hesapla birden fazla cihazda (ör. telefon ve web) aynı anda oturum açılabilir.
- Hatalı girişte hangi alanın yanlış olduğu söylenmez; genel bir "e-posta veya şifre hatalı" mesajı gösterilir.
- Art arda çok sayıda hatalı denemede giriş geçici olarak kısıtlanır.
- Girişte bekleyen metin onayı varsa (K58) uygulama açılmadan önce onay adımı gösterilir (HES-03).
**KUR-03 · Şifremi unuttum** · Mobil + Web
_Salon sahibi olarak şifremi unuttuğumda e-postama gelen linkle yeni şifre belirleyebilmek istiyorum._

- E-posta sistemde kayıtlı olsun ya da olmasın aynı mesaj gösterilir ("Kayıtlıysa e-posta gönderdik").
- Sıfırlama linki web sayfasında açılır (K14); yeni şifre belirlenince KUR-01'deki başarı ekranının aynısı gösterilir.
- Sıfırlama linki tek kullanımlıktır ve süreli çalışır.
- Şifre değişince diğer cihazlardaki oturumlar kapanır.

---

## İlk kurulum

**KUR-04 · Onboarding sihirbazı** · Mobil
_Salon sahibi olarak ilk girişte adım adım yönlendirilmek istiyorum, böylece 5 dakikada randevu almaya hazır olurum._

- Sihirbaz üç adımdan oluşur:
  1. İşletme bilgileri (KUR-05)
  2. Çalışma saatleri (KUR-06)
  3. Hizmetler (KUR-07)
- Üstte adım göstergesi bulunur ("1 / 3").
- Her adım atlanabilir.
- Sihirbaz bitince veya tamamen atlanınca sunucuda `onboardingCompletedAt` dolar ve sihirbaz bir daha açılmaz (K15).
- Eksik adımlar veriden türetilir, ayrıca saklanmaz:
  - İşletme bilgileri: salon telefonu veya adres boşsa eksik (K48)
  - Çalışma saatleri: varsayılan değerleri olduğu için hiçbir zaman eksik sayılmaz
  - Hizmetler: aktif hizmet yoksa eksik
- Eksik adım varsa takvimde "Kurulumu tamamla" kartı görünür; karta dokununca ilk eksik adımın ekranı açılır. Kart, eksik adım kalmayınca kendiliğinden kaybolur.
- Uygulama sihirbaz sırasında kapanırsa, tekrar açıldığında verisi eksik olan ilk adımdan devam eder. Aynı kural her cihazda aynı sonucu verir.
- Hizmetler adımında **hazır şablonlar** önerilir: Tıraş (Köpek), Yıkama + Kurutma (İkisi), Tırnak Kesimi (İkisi), Kulak Temizliği (İkisi). Süreleri ve türleri varsayılan olarak dolu gelir, owner fiyatları girer ve istemediği şablonu kaldırır.
- Sihirbaz bitince takvim açılır.

---

## İşletme ayarları

**KUR-05 · İşletme bilgileri** · Mobil + Web
_Salon sahibi olarak işletme adımı, telefonumu ve adresimi girip düzenleyebilmek istiyorum, böylece müşteriye giden mesajlarda doğru bilgiler görünür._

- Alanlar: işletme adı (zorunlu), salon telefonu (zorunlu), adres (zorunlu, K48), ilçe/şehir.
- Zorunluluk form kaydedilirken geçerlidir. Sihirbazda bu adım atlanırsa salon telefonu ve adres boş kalabilir; bu durum "Kurulumu tamamla" kartında görünür (KUR-04). İşletme adı ADM-01'de girildiği için hiçbir zaman boş olmaz.
- Salon telefonu Türkiye formatında doğrulanır ve "+90" ön ekiyle saklanır.
- Bu bilgiler hatırlatma şablonlarında, bakım raporunda, hizmet dökümünde ve intake formunda kullanılır. Adres, intake formundaki KVKK metninde veri sorumlusu adresi olarak geçer (INT-03).
**KUR-06 · Çalışma saatleri** · Mobil + Web
_Salon sahibi olarak hangi gün hangi saatler arasında açık olduğumu tanımlamak istiyorum, böylece takvim buna göre şekillenir._

- Her gün için açık/kapalı durumu ve tek bir saat aralığı girilir. Öğle arası yoktur (K13).
- Varsayılan: Pazartesi–Cumartesi 09:00–19:00, Pazar kapalı.
- Kapanış saati açılış saatinden önce olamaz.
- Çalışma saatleri dışı takvimde gri görünür. Randevu yine de alınabilir, sadece uyarı verilir (RAN-04).
- Çalışma saatleri değişince mevcut randevular etkilenmez.
**KUR-07 · Hizmet ekleme ve düzenleme** · Mobil + Web
_Salon sahibi olarak hizmetlerimi boyut kademesine göre süre ve fiyatla tanımlamak istiyorum, böylece randevuda doğru fiyat otomatik gelir._

- Alanlar: ad, **tür** (Köpek / Kedi / İkisi, K36) ve her kademe (küçük / orta / büyük) için ayrı süre ve fiyat.
- Tür "Kedi" seçilince "Tüm boyutlar için aynı" varsayılan olarak açık gelir; owner kapatabilir.
- **"Tüm boyutlar için aynı"** seçeneği vardır; seçilince tek süre ve tek fiyat girilir (ör. tırnak kesimi, kedi hizmetleri).
- Veri modeli: API her hizmet için **üç kademeyi her zaman dolu** tutar. "Tüm boyutlar için aynı" seçeneği yalnızca bir UI kolaylığıdır ve girilen değeri üç kademeye birden yazar. Seçenek ayrı bir alanda saklanmaz; düzenleme ekranında üç değer eşitse açık gösterilir.
- Süre 5 dakikalık adımlarla seçilir. Fiyat "450,00 ₺" formatında gösterilir.
- Hizmetler **yukarı/aşağı butonlarıyla** sıralanır. Bu sıra, randevu formundaki chip sırasını belirler. Sürükle-bırak MVP'de yok.
- Hizmet silinmez, **pasife alınır**. Pasif hizmet yeni randevuda görünmez; geçmiş randevu ve dökümlerde adı ve fiyatı değişmeden kalır.
- Hizmetin fiyatı değişince mevcut randevuların fiyatı değişmez.
**KUR-08 · Ek ücret kalemleri** · Mobil + Web
_Salon sahibi olarak "keçe açma", "huysuzluk ücreti" gibi ek ücretleri önceden tanımlamak istiyorum, böylece işlem sırasında tek dokunuşla eklerim (OPR-03)._

- Alanlar: ad ve varsayılan tutar.
- Ek ücret kalemi küçük ürün satışı için de kullanılır (şampuan, tasma). Ayrı ürün veya stok kavramı yoktur; dökümde ek ücret satırı olarak görünür (E7).
- Tutar randevuya eklenirken değiştirilebilir.
- Ek ücret randevu süresini değiştirmez.
- Hizmetlerde olduğu gibi silinmez, pasife alınır.
**KUR-09 · Özel kapalı günler** · Mobil + Web
_Salon sahibi olarak bayram veya izin gibi belirli tarihleri kapalı olarak işaretlemek istiyorum, böylece o günlere yanlışlıkla randevu vermem._

- Tek bir gün ya da tarih aralığı seçilebilir; isteğe bağlı bir açıklama eklenebilir (ör. "Kurban Bayramı").
- Kapalı gün takvimde gri ve etiketli görünür.
- Özel kapalı gün, haftalık kapalı bir güne (ör. Pazar) denk gelebilir. Bu durumda günde tek etiket görünür ve özel kapalı gün etiketi önceliklidir.
- O güne randevu girilmeye çalışılırsa uyarı verilir, kayıt engellenmez (RAN-04 ile aynı kural).
- Kapalı gün olarak işaretlenen tarihte zaten randevu varsa, owner'a bu randevuların listesi gösterilir.
- Geçmiş tarihli kapalı günler listede görünmez.

---

## Kapsam dışı

- Uygulama içinden kayıt olma, deneme süresi, abonelik ve ödeme ekranları
- SMS OTP, Google ile giriş, Apple ile giriş
- Çoklu şube
- Personel ve kullanıcı davet etme (Faz 2)
- Irk bazlı fiyatlandırma
- Öğle arası ve gün içinde birden fazla çalışma aralığı
- İşletmeye özel kilo eşikleri
- Universal link / app link ile linklerin uygulamada açılması
- Hizmetlerde sürükle-bırak sıralama

## Teknik notlar

| Konu | Karar | Story |
| -------------------- | --- | -------------- |
| Link açılışı | Davet ve sıfırlama linkleri web'de açılır. Uygulamada açılması universal link / app link ve dev build gerektirir, Expo Go'da çalışmaz. | KUR-01, KUR-03 |
| Web back office | Ayrı React uygulaması, MVP'de mobilden sonra (K47). Public web yüzeyi (K14) ondan bağımsızdır ve NestJS'te sunucu tarafında render edilir. | Tümü |
| İşletme durumu | `business.status: pilot \| active \| payment_due \| suspended \| deleting`. Askıda ve Silinecek için yazma istekleri `403 TENANT_SUSPENDED` ile reddedilir, okuma çalışır. `lib/api.ts` bu kodu tek yerde yakalar; yönlendirme yapmaz, oturum durumuna `suspended` bayrağı yazar. Şerit ve pasif butonlar bu bayrağı okur; şerit metni `status` alanına göre seçilir. İşletme yanıtında `status`, `graceEndsAt`, `suspendedAt`, `deleteAt` alanları bulunur. | ADM-02, HES-07 |
| Zamanlanmış görevler | Backend'de günlük bir job: (1) `graceEndsAt` geçen işletmeleri Askıda'ya alır, (2) `suspendedAt` + 11 ay olanlara silme uyarısı e-postası gönderir, (3) `suspendedAt` + 12 ay olanları ve `deleteAt` geçmiş olanları **aynı silme fonksiyonuyla** siler (HES-07), (4) `deleteAt` − 7 gün olanlara hatırlatma e-postası gönderir, (5) 30 günü dolan bekleyen intake formlarını siler (INT-04). E-posta gönderimi ADM-01'deki davet e-postasıyla aynı altyapıdır. | ADM-02, INT-04, HES-07 |
| Hizmet türü | `service.species: dog \| cat \| both`. Randevu formu hizmet listesini hayvanın türüne göre süzer; API yanlış türde satırı reddeder. | KUR-07, RAN-02 |
| Onboarding | Sunucuda yalnızca `business.onboardingCompletedAt`. Eksik adımlar veriden türetilir. | KUR-04 |
| Hizmet kademeleri | API her zaman üç kademe tutar; "tümü aynı" yalnızca UI'da. | KUR-07 |
| Sıralama | Yukarı/aşağı butonları; yeni kütüphane gerekmez. | KUR-07 |
| Onaylar | KUR-01'deki üç onay E8'deki `consent` tablosuna yazılır; `GET /me` bekleyen onayları döner (K58). | KUR-01, KUR-02 |

## Diğer epic'lere bağlantılar

| Buradan        | Oraya          | Konu                                             |
| -------------- | -------------- | ------------------------------------------------ |
| KUR-06, KUR-09 | RAN-04         | Çalışma saati ve kapalı gün uyarısı              |
| KUR-07         | HAY-01, RAN-02 | Boyut kademesi ve otomatik fiyat                 |
| KUR-08         | OPR-03         | İşlem sırasında ek ücret ekleme                  |
| KUR-05         | E3, E6, E7     | Salon bilgilerinin mesaj ve dökümlerde kullanımı |
| KUR-01, KUR-02 | HES-03, ADM-03 | Onay modeli ve yeniden onay                      |
| ADM-02         | HES-07         | Silinecek durumu ve ortak silme job'u            |
| K14            | E3, E6         | Public web yüzeyi intake formunu ve onay sayfasını barındırır |

## Açık sorular

- Yok.
