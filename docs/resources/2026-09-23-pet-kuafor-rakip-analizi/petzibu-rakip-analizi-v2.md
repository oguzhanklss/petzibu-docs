# Petzibu – Rakip Analizi v2

> Tarih: 23 Eylül 2026 · 21 rakip (8 global, 13 Türk)
> v1'e göre değişiklik: 9 yeni Türk rakip eklendi (PetKay, Patipu, Lumiperi, PawBooking, Planla, AtaksAPP, Marssoft, Kuaför Randevu, Rezzim). SalonAppy ve Salon Randevu bilgileri güncellendi. "Rakiplerden öğrenilecekler" bu dosyaya taşındı.
> Yöntem: Özellik ve fiyat bilgisi satıcıların kendi sitelerinden, yardım sayfalarından ve uygulama mağazalarından alındı. Şikâyetler Capterra, Trustpilot, App Store ve Şikâyetvar'dan. Doğrulanamayan her şey "?" olarak işaretli. Tahmin yok.

---

## 1. Özet

**Petzibu'nun boşluğu hâlâ açık, ama Türkiye'de pet'e özel rakip sandığımızdan kalabalık.** 5 yerli ürün doğrudan pet kuaförü hedefliyor: PetKay, Patipu, Lumiperi, PawBooking ve RandevuLS. Hiçbiri pazarda baskın değil ve hiçbiri Petzibu'nun MVP'sindeki pet modelini tam olarak kurmamış.

**En önemli 7 bulgu**

1. **Türk pet rakiplerinin hepsi erken aşamada ya da yan ürün.**
   - PetKay'in sitesinde yaklaşık 10 işletme listeleniyor.
   - Patipu beta sürümde (v0.2.86). "500+ işletme" iddiası doğrulanamıyor ve KVKK metni doldurulmamış şablon.
   - Lumiperi'nin arkasında bir yapı şirketi var ve müşteri sayısı açıklanmıyor.
   - PawBooking 50+ partner diyor, ama DNA'sı pet oteli.
   - RandevuLS 8 sektöre satılan bir şablon.
2. **Hiçbir Türk pet rakibinin grooming için native mobil uygulaması yok.** PetKay mobil uyumlu web, Patipu PWA, Lumiperi yalnızca tarayıcı, RandevuLS kiosk tablet. PawBooking'in uygulaması otel odaklı. Petzibu'nun "mobil öncelikli, tek elle" yaklaşımı ana farklılaşma olabilir.
3. **Online randevu 21 rakibin 20'sinde tam olarak var.** Marssoft'ta kısmen. Petzibu'da ise Faz 3'te. Bu, v1'deki bulgunun güçlenmiş hali.
4. **Petzibu MVP'sindeki 4 özellik Türk rakiplerin hiçbirinde doğrulanmadı:**
   - Çok hayvanlı randevu.
   - Aşı süresi dolma uyarısı.
   - Yapılandırılmış bakım raporu.
   - Ödeme anında sonraki randevu önerisi.
5. **WhatsApp hatırlatmasının fiyata dahil olması artık beklenti.** Lumiperi, Planla ve Rezzim mesajı paketin içine koyuyor. Lumiperi, Meta'nın mesaj ücretini de kendisi karşılıyor. Kota ve kredi modeli (PawBooking, AtaksAPP, Salon Randevu) şikâyet konusu.
6. **Fiyat bandı netleşti.**
   - Pet'e özel ürünler: 399–1.250 ₺/ay.
   - Genel salon ürünleri: 0–2.999 ₺/ay.
   - "Tek paket, her şey sınırsız" modeli yaygınlaşıyor: PetKay, Lumiperi ve Kuaför Randevu. Planla kademeleri randevu sayısına, Rezzim personel sayısına göre ayırıyor; özellikler her pakette aynı.
7. **Türkiye'deki şikâyetler özellikten çok güvenle ilgili:** hesabın kilitlenmesi, verilere erişememe, iptal edememe, habersiz çekim, ani zam. Bu şikâyetler SalonAppy, Kolay Randevu, Salon Randevu ve Planla için var. Yeni pet rakiplerinin bir kısmında KVKK metni bile eksik.

PetSmart bir yazılım değil, bir kuaför zinciri. Bu yüzden analize alınmadı.

---

## 2. Rakip haritası

| Grup                                     | Rakipler                                                                 | Petzibu için anlamı                                                          |
| ---------------------------------------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| **A. Türkiye, pet'e özel**               | PetKay, Patipu, Lumiperi PetCareOS, PawBooking Business, RandevuLS       | Doğrudan rakip. Aynı müşteriyi, aynı arama kelimesiyle hedefliyor.           |
| **B. Türkiye, genel ürün + pet sayfası** | Planla, Salon Randevu, AtaksAPP, Marssoft (pet shop)                     | "Pet kuaför programı" aramasında karşımıza çıkıyor, ama pet veri modeli yok. |
| **C. Türkiye, genel salon (referans)**   | SalonAppy / Kolay Randevu, Rezzim, RandevuNet, Kuaför Randevu            | Pet'e özel değil. Fiyat, UX ve güven konusunda çıtayı belirliyor.            |
| **D. Global**                            | MoeGo, DaySmart Pet, Teddy, GrooMore, Gingr, PetExec, Pawfinity, Animalo | Türkiye'de yoklar. Pet modeli ve özellik derinliği için referans.            |

---

## 3. Rakip profilleri

### 3.1 Türkiye

| Rakip                                                                          | Grup | Konumlanma                                                                             | Platform                                          | Ölçek                                              | Öne çıkan                                                                                               | Zayıf yanı                                                                              |
| ------------------------------------------------------------------------------ | ---- | -------------------------------------------------------------------------------------- | ------------------------------------------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| [PetKay](https://petkay.com/pet-kuaforleri-rezervasyon-sistemi)                | A    | Pet kuaför ve pet oteli rezervasyonu. Asıl işi NFC/QR akıllı künye.                    | Mobil uyumlu web                                  | Yaklaşık 10 işletme listeleniyor                   | Önce/sonra fotoğraflarından otomatik kolaj, "bakımı gelenler" listesi, hikâyeli demo sayfası, tek paket | Aşı takibi yok, KVKK linki yok, WhatsApp elle gönderiliyor                              |
| [Patipu](https://patipu.com/)                                                  | A    | Pet kuaför için randevu, CRM ve operasyon platformu                                    | Web + PWA, kiosk modu                             | 500+ işletme iddiası (doğrulanamadı); beta v0.2.86 | Müşteriye canlı durum linki, kademeli hatırlatma, kapora otomasyonu, walk-in kuyruğu                    | Önce/sonra fotoğrafı ve WhatsApp yalnızca Pro'da; KVKK metni şablon; şirket bilgisi yok |
| [Lumiperi PetCareOS](https://lumiperi.com/tr-tr/pet-kuafor-veteriner-programi) | A    | 7 sektöre satılan platformun pet versiyonu (kuaför, veteriner, otel, eğitim, pet shop) | Yalnızca tarayıcı                                 | Açıklanmıyor                                       | Tek fiyat, her şey sınırsız, WhatsApp API ücreti dahil, Excel ile içe aktarma, AI analiz                | Native uygulama yok; aşı uyarısı ve bakım raporu yok; arkasında bir yapı şirketi var    |
| [PawBooking Business](https://business.pawbooking.co/pricing)                  | A    | Pet oteli ağırlıklı rezervasyon yazılımı + tüketici pazaryeri                          | Web, widget, iOS/Android partner uygulaması       | 50+ partner, 1.000+ pet sahibi                     | Pazaryeri, ön ödeme ve no-show kuralları, rol bazlı yetki ve işlem kaydı, e-fatura                      | Kuaförlük ikincil; mesaj kotası ve aşım ücreti; "işletme/lokasyon" dili tutarsız        |
| [RandevuLS](https://www.lsyazilim.com/randevuls/sektor/pet-grooming)           | A    | 8 sektöre satılan şablon ürün; pet sayfası var                                         | Web, kiosk tablet                                 | Açıklanmıyor                                       | Irka göre süre, aşı ve huy notu, KVKK uyumlu dijital onam                                               | Yıllık peşin 15.000 ₺+; yerel işletme uygulaması yok                                    |
| [Planla](https://planla.co/pet-kuaforu-randevu-programi)                       | B    | Küçük işletme için randevu + hatırlatma + kasa                                         | iOS, Android, web                                 | 500+ işletme                                       | Çevrimdışı mod, her pakette sınırsız SMS/WhatsApp, online kapora, borç/alacak                           | Pet modeli yok; App Store'da "hobi projesi" ve habersiz çekim yorumları                 |
| [Salon Randevu](https://www.salonrandevu.app/pet-kuafor-randevu-uygulamasi)    | B    | Genel salon yazılımı; pet kuaför sayfası var                                           | iOS, Android, Huawei, web                         | 10 B+ indirme (Play)                               | WhatsApp eklentisi, adisyon, borç, e-imza, ücretsiz katman                                              | Pet modeli yok; fiyat sayfadan sayfaya değişiyor; ani zam şikâyeti                      |
| [AtaksAPP](https://www.ataks.app/pet-kuafor-programi/)                         | B    | Çok sektörlü randevu ve iş akışı yazılımı                                              | Web, iOS, Android                                 | Android'de 10+ indirme                             | Kalıcı ücretsiz paket, müşteri portalı (iptal/güncelleme), Zapier/webhook, CSV içe aktarma              | Pet modeli yok; kullanıcı sayısı çok düşük görünüyor                                    |
| [Marssoft](https://www.marssoft.com.tr/solutions)                              | B    | KOBİ SaaS portföyü; Pet Panel pet shop odaklı                                          | Web; uygulama iddiası doğrulanamadı               | 500+ işletme (tüm ürünler)                         | Pet shop kasası, stok ve e-arşiv tek yerde                                                              | Grooming tek bir madde; fiyat açıklanmıyor; ayrı pet sayfası yok                        |
| [SalonAppy / Kolay Randevu](https://www.kolayrandevu.com/)                     | C    | Güzellik salonu yazılımı + tüketici pazaryeri                                          | iOS, Android, web                                 | 5.000+ işletme, Play'de 4,8 (≈3.200 yorum)         | Pazaryeri, Instagram DM ve WhatsApp modülleri, ücretsiz katman (ayda 100 randevu)                       | Şikâyetvar: hesap kilitlenmesi, verilere erişememe, iptal edememe, habersiz çekim       |
| [Rezzim](https://rezzim.com/)                                                  | C    | Güzellik pazaryeri + salon programı                                                    | iOS (Watch, widget, Dynamic Island), Android, web | 93 salon, 127 bin+ randevu                         | Sesle/yazıyla AI randevu, kilit ekranı widget'ı, KDV dahil net fiyat                                    | Pet hedefi yok; tek geliştiricili, çok genç ürün                                        |
| [RandevuNet](https://randevunet.com/)                                          | C    | "Ömür boyu ücretsiz" randevu yazılımı                                                  | iOS, Android, web                                 | 810+ şube                                          | Önce WhatsApp, iletilmezse SMS; sesli onay araması; Instagram DM                                        | Pet desteği yalnızca veteriner notu; ayrı hayvan profili yok                            |
| [Kuaför Randevu](https://kuaforrandevu.com.tr/)                                | C    | Kuaför ve berber için basit online randevu                                             | Mobil uyumlu web                                  | Açıklanmıyor                                       | Tek fiyat (500 ₺), 30 gün deneme, kara liste, tatil günü bloklama                                       | Çok ince özellik seti; pet hedefi yok                                                   |

### 3.2 Global

| Rakip                                         | Konumlanma                                 | Platform                        | Ölçek                  | Öne çıkan                                              | Zayıf yanı                                           |
| --------------------------------------------- | ------------------------------------------ | ------------------------------- | ---------------------- | ------------------------------------------------------ | ---------------------------------------------------- |
| [MoeGo](https://www.moego.pet/)               | Grooming öncelikli, her şey bir arada      | Web, iOS, Android               | 10 bin+ işletme        | Smart Schedule, özelleştirilebilir rapor kartı, üyelik | Pahalı kademeler, SMS kotası, Android hataları       |
| [DaySmart Pet](https://www.daysmart.com/pet/) | Köklü grooming yazılımı (eski 123Pet)      | Web, masaüstü, iOS, Android     | 5 bin+ kuaför          | Düşük giriş fiyatı, borç takibi, referans fotoğrafı    | Eski arayüz, telefon desteği kaldırıldı              |
| [Teddy](https://www.tryteddy.com/)            | AI asistanlı grooming yazılımı             | Web; SMS/ses komutu             | 1 bin+ groomer         | Sınırsız SMS, "Ask Teddy", AI resepsiyonist            | Aşı ve rapor kartı sitede yok, ödeme yalnızca Square |
| [GrooMore](https://www.groomore.com/)         | Uygun fiyatlı grooming yazılımı            | Web, iOS, Android               | Açıklanmıyor           | Neredeyse her şey en ucuz pakette                      | Pansiyon yok, iPhone'da personel kısıtı              |
| [Gingr](https://www.gingrapp.com/)            | Pansiyon/kreş platformu + grooming modülü  | Web; markalı müşteri uygulaması | 5 bin+ tesis           | PreCheck, aşısı geçmişe randevu engeli, rapor kartı    | Personel uygulaması 1,6★, kurulum uzun               |
| [PetExec](https://www.petexec.net/)           | Pansiyon/kreş; Gingr ile aynı çatı altında | Web, iOS, Android               | Açıklanmıyor           | $19,99'a sınırsız SMS, takvimde uyarı ikonları         | Grooming akışları zayıf                              |
| [Pawfinity](https://www.pawfinity.com/)       | Sabit fiyatlı her şey bir arada            | Web + PWA                       | Açıklanmıyor           | Sınırsız personel ve fotoğraf, rota önerisi            | SMS ayrıca ücretli                                   |
| [Animalo](https://animalo.com/)               | Avrupa'da çok dilli, ucuz platform         | Web                             | Fransa merkezli; 7 dil | €9,99 grooming paketi, ülkeye göre yerelleştirme       | SMS yeni geldi, POS ve stok yok                      |

---

## 4. Özellik matrisi

**Semboller:** ✓ var · ◐ kısmi · ✗ yok · ? resmî kaynakta doğrulanamadı

"?" çoğu zaman "sitede anlatılmıyor" demek, "yok" demek değil. Genel salon ürünlerinde pet'e özel satırları ✗ olarak işaretledim; bu ürünlerde hayvan, müşteriden ayrı bir kayıt olarak görünmüyor.

Petzibu sütunu, özelliğin epics.md'de hangi epic'e ya da faza düştüğünü gösteriyor.

### 4.1 Türkiye

Sütun kısaltmaları: **PK** PetKay · **PT** Patipu · **LU** Lumiperi · **PB** PawBooking · **LS** RandevuLS · **PL** Planla · **SR** Salon Randevu · **RN** RandevuNet · **SA** SalonAppy · **RZ** Rezzim · **AT** AtaksAPP · **MS** Marssoft · **KR** Kuaför Randevu

#### Kurulum, müşteri ve hayvan

| Özellik                             | Petzibu                  | PK  | PT  | LU  | PB  | LS  | PL  | SR  | RN  | SA  | RZ  | AT  | MS  | KR  |
| ----------------------------------- | ------------------------ | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| İşletme mobil uygulaması (native)   | MVP                      | ◐   | ◐   | ✗   | ✓   | ◐   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ◐   | ✗   |
| Web paneli                          | MVP                      | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   |
| Boy, ırk ya da tüye göre fiyat/süre | MVP · E1 (boy)           | ?   | ?   | ◐   | ◐   | ◐   | ✗   | ?   | ?   | ?   | ✗   | ◐   | ?   | ✗   |
| Ek ücret kalemleri                  | MVP · E1                 | ?   | ◐   | ?   | ✓   | ?   | ?   | ?   | ?   | ?   | ?   | ?   | ?   | ✗   |
| Müşteri başına birden fazla hayvan  | MVP · E2                 | ?   | ◐   | ?   | ?   | ◐   | ✗   | ✗   | ◐   | ✗   | ✗   | ✗   | ◐   | ✗   |
| Uyarı etiketi (takvimde)            | MVP · E2, E4             | ◐   | ◐   | ?   | ◐   | ◐   | ✗   | ?   | ?   | ?   | ✗   | ◐   | ?   | ✗   |
| Aşı takibi ve süre uyarısı          | MVP · E2                 | ✗   | ◐   | ◐   | ◐   | ◐   | ✗   | ?   | ◐   | ?   | ✗   | ✗   | ◐   | ✗   |
| Fotoğraf geçmişi                    | MVP · E2                 | ✓   | ✓   | ✓   | ◐   | ?   | ✗   | ?   | ?   | ?   | ✗   | ✗   | ?   | ✗   |
| Dijital kayıt formu                 | MVP · E3                 | ✓   | ?   | ◐   | ◐   | ✓   | ?   | ?   | ?   | ?   | ?   | ◐   | ?   | ✗   |
| Onam / e-imza / KVKK rızası         | MVP · E3 (KVKK)          | ✗   | ◐   | ✓   | ◐   | ✓   | ?   | ✓   | ?   | ◐   | ◐   | ?   | ?   | ✗   |
| Başka yazılımdan veri aktarma       | MVP rehber · Faz 2 Excel | ?   | ◐   | ✓   | ?   | ✓   | ✓   | ?   | ✓   | ?   | ◐   | ✓   | ?   | ?   |

#### Takvim ve randevu

| Özellik                           | Petzibu  | PK  | PT  | LU  | PB  | LS  | PL  | SR  | RN  | SA  | RZ  | AT  | MS  | KR  |
| --------------------------------- | -------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Gün/hafta görünümü, sürükle-bırak | MVP · E4 | ◐   | ✓   | ✓   | ◐   | ?   | ◐   | ✓   | ✓   | ✓   | ✓   | ◐   | ?   | ◐   |
| Çok hayvanlı randevu              | MVP · E4 | ?   | ?   | ?   | ?   | ?   | ✗   | ?   | ◐   | ?   | ✗   | ✗   | ?   | ✗   |
| Tekrarlayan randevu               | Yok (K7) | ?   | ?   | ✓   | ◐   | ?   | ✓   | ?   | ?   | ?   | ?   | ✓   | ?   | ?   |
| Müşterinin online randevu alması  | Faz 3    | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ◐   | ✓   |
| Bekleme listesi                   | Faz 3    | ?   | ◐   | ✓   | ?   | ?   | ?   | ?   | ?   | ◐   | ?   | ?   | ?   | ?   |
| Check-in ve durum akışı           | MVP · E5 | ✓   | ✓   | ?   | ✓   | ?   | ?   | ?   | ✓   | ?   | ◐   | ?   | ?   | ?   |
| Fotoğraflı bakım raporu           | MVP · E5 | ◐   | ◐   | ◐   | ◐   | ?   | ✗   | ?   | ?   | ?   | ✗   | ✗   | ?   | ✗   |
| Ödemede sonraki randevu önerisi   | MVP · E5 | ?   | ?   | ?   | ?   | ?   | ?   | ?   | ?   | ?   | ?   | ?   | ?   | ?   |

#### İletişim ve pazarlama

| Özellik                            | Petzibu                      | PK     | PT     | LU      | PB     | LS  | PL  | SR        | RN  | SA  | RZ  | AT     | MS  | KR  |
| ---------------------------------- | ---------------------------- | ------ | ------ | ------- | ------ | --- | --- | --------- | --- | --- | --- | ------ | --- | --- |
| Otomatik randevu hatırlatması      | MVP · E6 (yarı otomatik)     | ◐      | ✓      | ✓       | ✓      | ✓   | ✓   | ✓         | ✓   | ✓   | ✓   | ✓      | ◐   | ◐   |
| WhatsApp ile müşteriye mesaj       | MVP elle · Faz 2 otomatik    | ◐ elle | ✓ Pro  | ✓ dahil | ✓ kota | ✓   | ✓   | ✓ eklenti | ✓   | ◐   | ✓   | ✓ kota | ◐   | ✗   |
| İki yönlü mesajlaşma               | Yok (WhatsApp'ta)            | ✗      | ✓      | ?       | ◐      | ?   | ?   | ?         | ◐   | ◐   | ◐   | ?      | ?   | ✗   |
| Rebook / bakım zamanı hatırlatması | MVP · E6                     | ◐      | ?      | ◐       | ◐      | ?   | ✗   | ?         | ◐   | ?   | ✗   | ✗      | ◐   | ✗   |
| Pazarlama kampanyaları             | Faz 3                        | ◐      | ✓      | ✓       | ✓      | ?   | ?   | ◐         | ?   | ◐   | ◐   | ◐      | ?   | ?   |
| Yorum / memnuniyet toplama         | Yok                          | ✓      | ?      | ◐       | ◐      | ?   | ✓   | ?         | ✓   | ✓   | ✓   | ◐      | ?   | ?   |
| Müşteri randevu sayfası / portal   | Faz 3 (MVP'de yalnız intake) | ✓      | ✓      | ✓       | ✓      | ✓   | ✓   | ✓         | ✓   | ✓   | ✓   | ✓      | ◐   | ✓   |
| Yapay zekâ                         | Yok                          | ✗      | ◐ plan | ✓       | ?      | ?   | ?   | ✓         | ◐   | ?   | ✓   | ?      | ◐   | ✗   |

#### Para

| Özellik                     | Petzibu                     | PK  | PT  | LU  | PB  | LS  | PL  | SR  | RN  | SA  | RZ  | AT  | MS  | KR  |
| --------------------------- | --------------------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Entegre ödeme / POS         | Yok (elle kayıt)            | ✗   | ◐   | ◐   | ✓   | ?   | ◐   | ?   | ◐   | ◐   | ✓   | ✓   | ✓   | ✗   |
| Kapora / no-show kuralı     | Faz 3                       | ?   | ✓   | ✓   | ✓   | ?   | ✓   | ?   | ◐   | ◐   | ?   | ◐   | ?   | ◐   |
| Hizmet dökümü / adisyon     | MVP · E7                    | ?   | ?   | ?   | ✓   | ?   | ?   | ✓   | ?   | ◐   | ✓   | ?   | ✓   | ?   |
| Müşteri borcu / veresiye    | MVP · E7                    | ?   | ?   | ?   | ?   | ?   | ✓   | ✓   | ✓   | ✓   | ✓   | ?   | ?   | ?   |
| Gider kaydı                 | MVP · E7                    | ?   | ?   | ✓   | ?   | ?   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ?   | ?   |
| Raporlama                   | MVP · E7 özet · Faz 2 detay | ✓   | ✓   | ✓   | ✓   | ◐   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ?   |
| Paket / seans / üyelik      | Yok                         | ?   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ?   | ?   |
| Hediye çeki / sadakat puanı | Yok                         | ?   | ✓   | ✓   | ◐   | ◐   | ?   | ✓   | ?   | ✓   | ◐   | ?   | ✓   | ?   |
| Perakende stok              | Yok                         | ?   | ✓   | ✓   | ◐   | ?   | ✓   | ✓   | ✓   | ◐   | ✓   | ?   | ✓   | ?   |

#### Personel, ölçek ve uyum

| Özellik                         | Petzibu                     | PK          | PT       | LU     | PB                | LS     | PL  | SR  | RN     | SA  | RZ     | AT  | MS        | KR     |
| ------------------------------- | --------------------------- | ----------- | -------- | ------ | ----------------- | ------ | --- | --- | ------ | --- | ------ | --- | --------- | ------ |
| Personel, rol ve yetki          | Faz 2                       | ◐           | ✓        | ◐      | ✓                 | ✓      | ✓   | ✓   | ✓      | ✓   | ✓      | ✓   | ◐         | ◐      |
| Prim / komisyon                 | Faz 2                       | ?           | ✓        | ✓      | ?                 | ✓      | ?   | ✓   | ◐      | ✓   | ✓      | ?   | ?         | ?      |
| Bordro / mesai / izin           | Faz 2                       | ?           | ◐        | ◐      | ?                 | ?      | ?   | ?   | ?      | ?   | ◐      | ?   | ?         | ?      |
| Mobil kuaför rotası             | Yok                         | ?           | ?        | ✗      | ◐                 | ?      | ✗   | ?   | ?      | ?   | ✗      | ✗   | ✗         | ✗      |
| Çoklu şube                      | Yok (veri modeli hazır, K5) | ?           | ✓        | ✓      | ✓                 | ✓      | ?   | ✓   | ✓      | ✓   | ✓      | ✓   | ◐         | ?      |
| Pet oteli / kreş                | Yok                         | ✓ ayrı ürün | ?        | ◐      | ✓                 | ◐      | ✗   | ?   | ?      | ?   | ✗      | ✗   | ?         | ✗      |
| Veri dışa aktarma / entegrasyon | MVP · E8                    | ?           | ✓        | ◐      | ✓                 | ◐      | ◐   | ?   | ✓      | ◐   | ?      | ✓   | ◐         | ?      |
| KVKK metni / e-fatura           | MVP · KVKK                  | ✗           | ◐ şablon | ◐ KVKK | ✓ KVKK + e-fatura | ◐ KVKK | ?   | ?   | ◐ KVKK | ?   | ◐ KVKK | ?   | ✓ e-arşiv | ◐ KVKK |

### 4.2 Global

Sütun kısaltmaları: **MG** MoeGo · **DS** DaySmart · **TD** Teddy · **GM** GrooMore · **GI** Gingr · **PE** PetExec · **PF** Pawfinity · **AN** Animalo

| Özellik                            | Petzibu          | MG  | DS  | TD  | GM  | GI  | PE  | PF     | AN                   |
| ---------------------------------- | ---------------- | --- | --- | --- | --- | --- | --- | ------ | -------------------- |
| İşletme mobil uygulaması           | MVP              | ✓   | ✓   | ◐   | ✓   | ◐   | ✓   | ◐      | ◐                    |
| Boy, ırk ya da tüye göre fiyat     | MVP · E1 (boy)   | ✓   | ?   | ?   | ✓   | ◐   | ◐   | ✓      | ✓                    |
| Ek ücret kalemleri                 | MVP · E1         | ✓   | ◐   | ✓   | ?   | ✓   | ◐   | ✓      | ◐                    |
| Müşteri başına birden fazla hayvan | MVP · E2         | ✓   | ✓   | ◐   | ✓   | ✓   | ✓   | ✓      | ✓                    |
| Uyarı etiketi (takvimde)           | MVP · E2, E4     | ◐   | ◐   | ◐   | ◐   | ?   | ✓   | ◐      | ◐                    |
| Aşı takibi ve süre uyarısı         | MVP · E2         | ✓   | ✓   | ?   | ✓   | ✓   | ✓   | ◐      | ✓                    |
| Fotoğraf geçmişi                   | MVP · E2         | ◐   | ✓   | ◐   | ?   | ✓   | ◐   | ✓      | ◐                    |
| Dijital kayıt formu                | MVP · E3         | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓      | ✓                    |
| Onam / e-imza                      | Yok              | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓      | ◐                    |
| Gün/hafta görünümü, sürükle-bırak  | MVP · E4         | ✓   | ✓   | ✓   | ◐   | ✓   | ✓   | ◐      | ◐                    |
| Çok hayvanlı randevu               | MVP · E4         | ✓   | ✓   | ?   | ?   | ✓   | ?   | ◐      | ?                    |
| Tekrarlayan randevu                | Yok (K7)         | ✓   | ✓   | ?   | ✓   | ✓   | ◐   | ✓      | ?                    |
| Müşterinin online randevu alması   | Faz 3            | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓      | ✓                    |
| Bekleme listesi                    | Faz 3            | ✓   | ✓   | ✓   | ✓   | ✓   | ◐   | ◐      | ?                    |
| Check-in ve durum akışı            | MVP · E5         | ✓   | ◐   | ✓   | ✓   | ✓   | ✓   | ?      | ◐                    |
| Fotoğraflı bakım raporu            | MVP · E5         | ✓   | ?   | ?   | ?   | ✓   | ◐   | ✓      | ✓                    |
| Ödemede sonraki randevu önerisi    | MVP · E5         | ◐   | ?   | ?   | ?   | ?   | ?   | ?      | ?                    |
| Otomatik hatırlatma                | MVP · E6         | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓      | ✓                    |
| WhatsApp                           | MVP elle · Faz 2 | ?   | ?   | ?   | ?   | ?   | ?   | ?      | ?                    |
| İki yönlü mesajlaşma               | Yok              | ✓   | ✓   | ✓   | ✓   | ◐   | ✓   | ✓      | ◐                    |
| Rebook / aşı hatırlatması          | MVP · E6         | ◐   | ◐   | ◐   | ◐   | ◐   | ◐   | ?      | ✓                    |
| Pazarlama                          | Faz 3            | ✓   | ✓   | ◐   | ✓   | ✓   | ✓   | ✓      | ◐                    |
| Yorum toplama                      | Yok              | ✓   | ✓   | ✓   | ✓   | ✓   | ◐   | ✓      | ?                    |
| Entegre ödeme / POS                | Yok              | ✓   | ✓   | ✓   | ✓   | ✓   | ✓   | ✓      | ◐                    |
| Kapora / no-show                   | Faz 3            | ✓   | ✓   | ◐   | ✓   | ◐   | ◐   | ◐      | ✓                    |
| Müşteri borcu                      | MVP · E7         | ?   | ✓   | ?   | ?   | ✓   | ✓   | ◐      | ?                    |
| Gider kaydı                        | MVP · E7         | ?   | ◐   | ?   | ?   | ?   | ?   | ✓      | ?                    |
| Paket / üyelik                     | Yok              | ✓   | ✓   | ?   | ?   | ✓   | ✓   | ✓      | ✓                    |
| Perakende stok                     | Yok              | ✓   | ✓   | ◐   | ✓   | ✓   | ✓   | ✓      | ?                    |
| Personel, rol, prim                | Faz 2            | ✓   | ✓   | ✓   | ✓   | ✓   | ◐   | ✓      | ◐                    |
| Mobil kuaför rotası                | Yok              | ✓   | ✓   | ✓   | ✓   | ?   | ?   | ✓      | ?                    |
| Pansiyon / kreş                    | Yok              | ✓   | ✓   | ✗   | ✗   | ✓   | ✓   | ✓      | ✓                    |
| Yapay zekâ                         | Yok              | ◐   | ?   | ✓   | ✗   | ?   | ?   | ?      | ?                    |
| Yerel uyum                         | MVP · KVKK       | ✗   | ✗   | ✗   | ✗   | ?   | ?   | ◐ GDPR | ◐ GDPR + FR e-fatura |

---

## 5. Fiyatlandırma

Türkiye'de pet'e özel ürünlerin giriş fiyatı 399–1.250 ₺/ay. Genel ürünlerin çoğunda ücretsiz katman var. Mesajın pakete dahil olup olmadığı fiyat kadar önemli.

### 5.1 Türkiye

| Rakip                                                                                      | Model                                             | Fiyat (aylık)                                                                                            | Mesaj                                                  | Deneme               |
| ------------------------------------------------------------------------------------------ | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ | -------------------- |
| [Patipu](https://patipu.com/)                                                              | 3 kademe, KDV hariç                               | 399 ₺ (200 randevu, 2 personel, yalnız SMS) · 799 ₺ (sınırsız, WhatsApp, foto) · 1.599 ₺ (çok şube, API) | Belirtilmemiş; Pro'da kendi sağlayıcını bağlama        | 14 gün, kartsız      |
| [PetKay](https://petkay.com/pet-kuaforleri-rezervasyon-sistemi)                            | Tek paket, KDV hariç                              | 900 ₺ veya 7.500 ₺/yıl                                                                                   | Belirtilmemiş                                          | İlk ay ücretsiz      |
| [PawBooking](https://business.pawbooking.co/pricing)                                       | 4 kademe; KDV belirtilmemiş                       | 1.200 ₺ · 2.100 ₺ · 6.375 ₺ · teklifle (yıllıkta %25 indirim)                                            | Kota: 200–1.500 bildirim/ay, aşımda 0,15–0,25 ₺/mesaj  | 30 gün para iadesi   |
| [Lumiperi](https://lumiperi.com/tr-tr/pet-kuafor-veteriner-programi)                       | Tek paket, KDV hariç                              | 1.250 ₺ veya 12.500 ₺/yıl                                                                                | WhatsApp API dahil, Meta ücreti de dahil               | 7 gün, kartsız       |
| [RandevuLS](https://www.lsyazilim.com/randevuls/paketler)                                  | Yıllık lisans, KDV hariç                          | 15.000 / 20.000 / 30.000 ₺/yıl                                                                           | NetGSM üzerinden, fiyatı açıklanmamış                  | Demo                 |
| [Kuaför Randevu](https://kuaforrandevu.com.tr/)                                            | Tek paket                                         | 500 ₺                                                                                                    | SMS isteğe bağlı, salon NetGSM'e kendi öder            | 30 gün, kartsız      |
| [AtaksAPP](https://www.ataks.app/fiyatlar/)                                                | 3 kademe                                          | Ücretsiz · 799 ₺ · 999 ₺                                                                                 | Ücretli paketlerde 1.000 WhatsApp mesajı               | 30 gün, kartsız      |
| [Salon Randevu](https://www.salonrandevu.app/paket-liste.php)                              | 4 kademe, KDV hariç                               | Ücretsiz · 599 ₺ · 949 ₺ · teklifle                                                                      | SMS paketi (500 SMS 115 ₺); WhatsApp eklentisi 79 ₺/ay | Ücretsiz katman      |
| [RandevuNet](https://randevunet.com/)                                                      | 2 kademe                                          | Ücretsiz (reklamlı) · 417 ₺ (yıllık) / 499 ₺ (aylık)                                                     | Ücretsizde kredi, Pro'da sınırsız                      | Ücretsiz katman      |
| [Planla](https://planla.co/pet-kuaforu-randevu-programi/fiyatlar)                          | Randevu sayısına göre; tüm özellikler her pakette | 1.190 ₺ (100 randevu) · 1.690 ₺ (300) · 2.690 ₺ (sınırsız)                                               | Sınırsız SMS/WhatsApp dahil                            | 14 gün koşulsuz iade |
| [Rezzim](https://rezzim.com/fiyatlandirma)                                                 | Personel sayısına göre; KDV dahil                 | 1.499 ₺ · 1.999 ₺ · 2.999 ₺                                                                              | SMS ve WhatsApp dahil                                  | 14 gün, kartsız      |
| [SalonAppy](https://apps.apple.com/tr/app/salonappy-salon-y%C3%B6netimi/id1097504667?l=tr) | Ücretsiz katman + 3 kademe                        | Ayda 100 randevuya kadar ücretsiz · App Store'da 1.499 / 2.199 / 3.249,99 ₺                              | Bilinmiyor                                             | Ücretsiz katman      |
| [Marssoft](https://www.marssoft.com.tr/pricing)                                            | 3 kademe                                          | Teklifle                                                                                                 | Bilinmiyor                                             | 14 gün               |

Fiyat notları:

- **SalonAppy:** Elimizdeki 1.299 / 1.699 / 2.599 ₺ rakamları doğrulanamadı; site 403 veriyor. Bu rakamlar Marssoft'un SalonPro.io ürününün fiyatlarıyla birebir aynı, karışıklık olabilir. Tabloda App Store fiyatları var; dönemleri belirtilmemiş.
- **Salon Randevu:** Pet kuaför sayfasında daha düşük fiyatlar yazıyor (299 / 599 / 849 ₺). Tabloda paket listesindeki rakamlar var.
- **PawBooking:** Fiyatların aylık ödemeye mi, yıllık ödemenin aylık karşılığına mı ait olduğu net değil.

### 5.2 Global

| Rakip                                                 | Giriş                 | Üst                       | Mesaj                         | Deneme              |
| ----------------------------------------------------- | --------------------- | ------------------------- | ----------------------------- | ------------------- |
| [MoeGo](https://www.moego.pet/pricing)                | $79 salon / $49 mobil | $239; Enterprise teklifle | Kota: 300–1.350 SMS           | Belirtilmemiş       |
| [DaySmart Pet](https://www.daysmart.com/pet/pricing/) | $29                   | $199; teklifle            | Kota: 500–5.000 SMS           | 14 gün              |
| [Teddy](https://www.tryteddy.com/pricing)             | $49                   | $199                      | Sınırsız SMS                  | 14 gün              |
| [GrooMore](https://www.groomore.com/pricing.html)     | $49                   | $79; teklifle             | Kota: 400–800 SMS             | Kaynaklar çelişiyor |
| [Gingr](https://www.gingrapp.com/pricing)             | $109 (Spa)            | $209; teklifle            | Eklenti                       | Belirtilmemiş       |
| [PetExec](https://www.petexec.net/purchase-petexec)   | ≈$105                 | Tek paket                 | Sınırsız SMS eklentisi $19,99 | 30 gün para iadesi  |
| [Pawfinity](https://www.pawfinity.com/pricing/)       | $60                   | $110                      | $0,05/SMS                     | 7 gün               |
| [Animalo](https://animalo.com/en/for_grooming)        | €9,99                 | €19,99                    | Ön ödemeli kredi              | 30 gün              |

---

## 6. Modül bazında rakiplerden öğrenilecekler

Bu bölüm yalnızca epics.md'deki mevcut E1–E8 modüllerini kapsıyor. Yeni modül önerileri 7. bölümde.

**Öncelik etiketleri**

- **MVP:** Az emekle mevcut epic'e eklenebilir.
- **Tasarım notu:** Şimdi kodlanmasa da veri modelinde ya da UX'te şimdiden hesaba katılmalı.
- **Sonra:** Faz 2 veya sonrası.

### E1 · Kurulum & İşletme

| Rakip ne yapıyor                                                                                                                  | Petzibu için öneri                                                                                                                                                                       | Öncelik          |
| --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- |
| GrooMore ve PetExec hayvana özel fiyat tutuyor.                                                                                   | Hayvanın son fiyatı hatırlansın ve sonraki randevuda önerilsin. Kademe fiyatı "Paşa hep 500 ₺ öder" durumunu karşılamıyor.                                                               | MVP              |
| Kuaför Randevu tatil ve kapalı günleri takvimde bloklatıyor.                                                                      | Çalışma saatlerine tek seferlik kapalı gün/tatil eklenebilsin. Online randevu gelince bu zorunlu olacak.                                                                                 | MVP              |
| Lumiperi ırka göre varsayılan süre ve tüy/tıraş tercihi tutuyor. Gingr ırk, kilo ve çoklu hayvan indirimi kuralları kullanıyor.   | MVP'de yalnızca boy kademesi kalsın. Ama fiyat ve süre hizmet tablosunda sabit bir sütun olarak değil, **kural** olarak modellensin; ırk veya çoklu hayvan kuralı sonradan eklenebilsin. | Tasarım notu     |
| PetKay'in satış sayfası, müşteri ve işletme tarafını bir hikâyeyle adım adım gösteriyor ("Merve, Ponçik için randevu alıyor").    | Onboarding ve satış sayfası için aynı yaklaşım: owner'a ilk 5 dakikada "telefonla gelen randevu" senaryosu oynatılsın.                                                                   | MVP (onboarding) |
| Lumiperi Excel şablonu ile, Rezzim telefon rehberinden tek dokunuşla müşteri aktarıyor. MoeGo ve Teddy taşımayı ücretsiz yapıyor. | Rehberden aktarma zaten MVP'de. Faz 2'deki Excel aktarımı hazır bir şablonla gelsin ve rakiplerin dışa aktarım dosyalarını kabul etsin.                                                  | Sonra            |

### E2 · Müşteri & Hayvan

| Rakip ne yapıyor                                                                                  | Petzibu için öneri                                                                                              | Öncelik |
| ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ------- |
| DaySmart'ta önce/sonra fotoğraflarından ayrı bir referans fotoğrafı var.                          | Profilde sabitlenen "istenen kesim" fotoğrafı. Groomer değişse de sonuç tutarlı olur.                           | MVP     |
| PawBooking tekrar randevuda önceki bakım notlarını otomatik açıyor.                               | Yeni randevu açılınca son ziyaretin notu ve son kesim fotoğrafı randevu detayında görünsün.                     | MVP     |
| DaySmart aşı uyarısını ödeme ekranında da gösteriyor. MoeGo takvimde renkli ikon kullanıyor.      | Aşı durumu takvimde, randevu detayında ve tahsilat ekranında görünsün.                                          | MVP     |
| PetExec sahibe de uyarı koyuyor ("geç kalır", "ödemede sorun"). Kuaför Randevu'da kara liste var. | Müşteri etiketleri takvim kartında da görünsün. "Kara liste" etiketli müşteriye randevu açılırken uyarı çıksın. | MVP     |
| AtaksAPP'ta alerji ve tercih notları personel yetkisine göre görünüyor.                           | Faz 2'deki personel modülünde notlar role göre görünsün.                                                        | Sonra   |

### E3 · Intake Form

| Rakip ne yapıyor                                                                                               | Petzibu için öneri                                                                                                   | Öncelik      |
| -------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ------------ |
| Gingr PreCheck, randevudan önce mevcut müşteriye "bilgilerini güncelle, aşı belgesini yükle" linki gönderiyor. | Intake linki mevcut müşteriye de gönderilebilsin. Aşı karnesi fotoğrafı alanı açıkça bulunsun.                       | MVP          |
| PetKay forma zorunlu/opsiyonel özel sorular eklettiriyor ("makastan ürküyor mu?").                             | K6'daki alan açıp kapatmaya "zorunlu" seçeneği eklensin. Form builder'a gerek yok.                                   | MVP          |
| Lumiperi, RandevuLS, MoeGo ve Gingr formda dijital onay alıyor. PetKay'de KVKK linki bile yok.                 | Bakım riski onayı (keçe, kısa tıraş, yaşlı hayvan) onay kutusu olarak eklensin. KVKK metni gerçek ve eksiksiz olsun. | MVP          |
| GrooMore'da sorular türe ve ırka göre değişiyor.                                                               | Sabit şablonda en azından kedi/köpek ayrımı olsun.                                                                   | MVP          |
| Patipu kapora, iptal süresi ve aşı şartını formun sonunda değil, ilgili adımda gösteriyor.                     | Intake'te ve ileride online randevuda kurallar karar anında görünsün.                                                | Tasarım notu |

### E4 · Takvim & Randevu

| Rakip ne yapıyor                                                                                                                | Petzibu için öneri                                                                                               | Öncelik      |
| ------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------ |
| MoeGo Smart Schedule boşluk bırakmayan saati öneriyor.                                                                          | Randevu açılırken hizmet süresine sığan ilk boş saat önerilsin. Tasarımdaki "Müsait Saat" blokları bunun temeli. | MVP          |
| Takvim satırında 5 global ve 6 Türk rakip tam puan alıyor; çoğunda sürükle-bırak ile erteleme var.                              | Takvimde kartı sürükleyerek erteleme.                                                                            | MVP          |
| Patipu'da en ucuz pakette bile walk-in / aynı gün kuyruğu var.                                                                  | Randevusuz gelen müşteri için "şimdi" butonu: saat seçmeden anlık randevu.                                       | MVP          |
| Planla çevrimdışı çalışıyor.                                                                                                    | Yıkama alanında sinyal zayıf olabilir. En azından bugünün takvimi ve randevu detayları çevrimdışı okunabilsin.   | Tasarım notu |
| Rezzim'de sesle/yazıyla AI randevu (Watch dahil), kilit ekranı widget'ı (bugünkü ciro, sıradaki randevu) ve Dynamic Island var. | Islak elle çalışan groomer için güçlü fikirler. MVP'ye değil, sonraya.                                           | Sonra        |
| GrooMore'da ajanda (liste) görünümü var.                                                                                        | Mobilde saat ızgarasına ek olarak günün listesi.                                                                 | Sonra        |

### E5 · Randevu Operasyonu

| Rakip ne yapıyor                                                                                   | Petzibu için öneri                                                                                                                                                | Öncelik                    |
| -------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| Gingr ve MoeGo'da rapor kartı yapılandırılmış alanlardan oluşuyor. Türk rakiplerin hiçbirinde yok. | Rapora seçilebilir alanlar: davranış (sakin/huzursuz), cilt/kulak/tüy bulgusu, sonraki bakım önerisi. "Huzursuzdu" iki kez işaretlenirse uyarı etiketi önerilsin. | MVP                        |
| PetKay önce/sonra fotoğraflarından otomatik kolaj yapıyor.                                         | Rapor paylaşılırken önce/sonra tek görsel halinde birleştirilsin. Hem müşteriye gider hem salonun Instagram'ı için hazır içerik olur.                             | MVP                        |
| Gingr hayvan hazır olunca teslim alma bildirimi gönderiyor.                                        | "Paşa hazır, gelip alabilirsiniz" WhatsApp şablonu, tek dokunuş.                                                                                                  | MVP                        |
| Patipu müşteriye canlı durum linki veriyor (Sırada → Geldi → Banyoda → Hazır).                     | "Ne zaman biter?" aramalarını keser. Hesapsız link altyapısıyla yapılabilir.                                                                                      | Sonra                      |
| MoeGo, müşteri sonraki randevuyu almadıysa rapora randevu linki ekliyor.                           | Rapor mesajına rebook önerisi metni eklensin. Online randevu gelince bu bir linke dönüşür.                                                                        | MVP (metin) / Sonra (link) |

### E6 · Hatırlatmalar

| Rakip ne yapıyor                                                                                                                                                       | Petzibu için öneri                                                                                                                                                     | Öncelik |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| DaySmart'ta müşteri SMS'e "C" yazarak onaylıyor. Salon Randevu onay linkini 79 ₺/ay eklenti olarak satıyor.                                                            | Hatırlatma mesajına tek tıkla onay linki. Müşteri tıklayınca randevu "Onaylandı" olur. Yarı otomatik modelin en büyük eksiğini WhatsApp API'ye gerek kalmadan kapatır. | MVP     |
| PetKay'in "bakımı gelenler" listesi, yaklaşık 1 ay önce gelmiş müşterileri süzüp tek tıkla şablon gönderiyor. Teddy, GrooMore ve Animalo da pasif müşteriyi yakalıyor. | "X haftadır gelmeyenler" listesi. Rebook tarihi kaydedilmemiş müşterileri de kapsar.                                                                                   | MVP     |
| Patipu kademeli hatırlatma gönderiyor (aynı gün, 1 gün önce, 1 saat önce). RandevuNet iki aşamalı.                                                                     | Zamanlama ayarı birden fazla hatırlatmaya izin versin.                                                                                                                 | MVP     |
| PawBooking randevu öncesi "getirilecekler" mesajı gönderiyor (ör. aşı karnesi).                                                                                        | Aşı durumu "Bilinmiyor" veya "Süresi geçmiş" ise hatırlatma mesajına "aşı karnesini getirin" satırı eklensin.                                                          | MVP     |
| Rakipler hatırlatmayı tamamen otomatik gönderiyor.                                                                                                                     | Yarı otomatik modelde günde 8–10 bildirim owner'ı yorabilir. Tek tek bildirim yerine "Bugün gönderilecek 8 mesaj" kuyruğu, sırayla aç-gönder.                          | MVP     |
| Patipu başarısız gönderimleri tekrar deneme kuyruğuna alıyor. RandevuNet WhatsApp iletilmezse SMS'e düşüyor.                                                           | Faz 2'deki otomatik WhatsApp bu mantıkla tasarlansın.                                                                                                                  | Sonra   |

### E7 · Tahsilat & Kasa

| Rakip ne yapıyor                                                                                 | Petzibu için öneri                                                                                                                        | Öncelik      |
| ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| GrooMore kullanıcıları dökümün ancak ödemeden sonra gönderilebilmesinden şikâyetçi.              | Döküm tahsilattan önce de paylaşılabilsin.                                                                                                | MVP          |
| Gingr içe aktarmada açılış borç bakiyesini de taşıyor.                                           | Müşteri eklenirken açılış borcu girilebilsin. Deftere veresiye yazan kuaför için kritik.                                                  | MVP          |
| Lumiperi gün sonu Z raporu veriyor.                                                              | Aylık özetin yanına "bugün" kasa özeti: nakit, kart, havale, açık kalan borç.                                                             | MVP          |
| Marssoft pet shop kasasını grooming ile aynı yerde tutuyor.                                      | Salonun küçük bir ürün köşesi varsa dökümde "ürün" satırı olsun (şampuan, tasma). Stok takibi olmadan, ek ücret kalemi gibi.              | MVP          |
| Pawfinity müşteri alacağını değişiklik kaydıyla tutuyor.                                         | Borç bakiyesi eksiye düşebilsin (fazla ödeme, ön ödeme). Paket ve seans kartı bunun üzerine kurulur.                                      | Tasarım notu |
| PetExec kullanıcılarının en büyük şikâyeti komisyonun bakım gününe değil ödeme gününe yazılması. | Faz 2'deki primin hizmet tarihine mi tahsilat tarihine mi yazılacağı şimdiden kararlaştırılsın. K2 gereği gelir tahsilat anında oluşuyor. | Tasarım notu |

### E8 · Hesap & Veri

| Rakip ne yapıyor                                                                                               | Petzibu için öneri                                                                         | Öncelik                                |
| -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ | -------------------------------------- |
| SalonAppy ve Kolay Randevu'nun Şikâyetvar'daki ana şikâyeti: hesap kilitlenince müşteri verisine erişilemiyor. | Abonelik bitse de veri salt okunur kalsın ve dışa aktarılabilsin. Bunu açıkça vaat edelim. | MVP                                    |
| Kolay Randevu ve Planla'da iptal zorluğu ve habersiz çekim şikâyetleri var. Salon Randevu'da ani zam.          | Uygulama içinden tek dokunuşla iptal. Zam için önceden haber verme sözü.                   | MVP (iptal) / Sonra (fiyat politikası) |
| Patipu'nun KVKK metni doldurulmamış şablon, PetKay'de hiç yok. RandevuLS ve Lumiperi KVKK'yı öne çıkarıyor.    | KVKK zaten MVP'de. Satış sayfasında görünür yapılsın.                                      | —                                      |

---

## 7. Petzibu için öneriler

### 7.1 MVP'ye eklenmesi önerilenler

Hepsi mevcut epic'lerin içinde, az emekle yapılabilir. Etki sırasına göre:

| #   | Ekleme                                                               | Epic   | Neden                                                                       |
| --- | -------------------------------------------------------------------- | ------ | --------------------------------------------------------------------------- |
| 1   | Hatırlatmaya tek tıkla onay linki                                    | E6     | Yarı otomatik modelin yanıt okuyamama sorununu çözer                        |
| 2   | Yapılandırılmış bakım raporu + önce/sonra kolajı                     | E5     | Türk rakiplerin hiçbirinde tam hali yok; salona Instagram içeriği de üretir |
| 3   | "X haftadır gelmeyenler" listesi                                     | E6     | Doğrudan gelir; PetKay'in en iyi fikri                                      |
| 4   | Toplu hatırlatma kuyruğu                                             | E6     | Yarı otomatik modelin owner'ı yormasını önler                               |
| 5   | Son ziyaret notu ve referans fotoğrafı randevuda                     | E2, E4 | Groomer'ın en sık baktığı bilgi                                             |
| 6   | Hayvana özel fiyat hafızası                                          | E1, E4 | Kademe fiyatının boşluğunu kapatır                                          |
| 7   | Intake linkini mevcut müşteriye gönderme + aşı karnesi fotoğrafı     | E3     | Aşı verisi kendiliğinden güncel kalır                                       |
| 8   | Bakım riski onayı ve gerçek KVKK metni                               | E3     | Hukuki koruma; rakiplerin açığı                                             |
| 9   | Walk-in "şimdi" randevusu, sürükle-bırak erteleme, boş saat önerisi  | E4     | Günlük hız                                                                  |
| 10  | "Hazır" mesajı, açılış borcu, günlük kasa özeti, dökümde ürün satırı | E5, E7 | Küçük ama her gün kullanılan işler                                          |
| 11  | Veri erişim garantisi ve tek dokunuşla iptal                         | E8     | Türkiye'deki en büyük şikâyet konusu güven                                  |

### 7.2 Faz sırası için karar gerekenler

1. **Online randevu Faz 3'ten Faz 2'ye çekilmeli.** 21 rakibin 20'sinde var. Ucuz yolu: "talep gönder, salon onaylasın" modeli (Teddy, Gingr). E3'teki hesapsız link ve onay kuyruğu burada yeniden kullanılır; slot bazlı müsaitlik gerekmez. Randevu linki Instagram bio'suna konabilir (PawBooking, Planla).
2. **Paket ve seans kartı Faz 2'ye girmeli.** 21 rakibin 16'sında var. Eksiye düşebilen borç bakiyesinin üzerine kurulabilir.
3. **Faz 2'deki otomatik WhatsApp'ın maliyeti fiyata dahil edilmeli.** Lumiperi, Planla ve Rezzim böyle yapıyor. Kota ve kredi modeli rakiplerde şikâyet konusu.

### 7.3 Farklılaşma

- **Native mobil, tek elle kullanım.** Türk pet rakiplerinin hiçbirinde grooming için native uygulama yok.
- **Pet veri modeli + Türkiye'ye özgü finans bir arada.** Çok hayvanlı randevu, aşı süresi uyarısı, borç/veresiye ve gider birlikte hiçbir rakipte doğrulanmadı.
- **Sonraki randevu önerisi ve yapılandırılmış rapor.** Rebook oranı doğrudan ciroya döner.
- **Güven ve şeffaflık.** Net fiyat (KDV dahil yazılmış), mesaj dahil, veri garantisi, gerçek KVKK metni, kolay iptal.

### 7.4 İş modeli için sinyaller (karar değil, girdi)

- Pet'e özel ürünlerin giriş fiyatı 399–1.250 ₺/ay. Ücretsiz katmanlı genel ürünler de var.
- En sade ve en çok övülen modeller: tek paket (PetKay, Lumiperi, Kuaför Randevu) veya tek bir değişkene göre kademe (Planla randevu sayısına, Rezzim personel sayısına göre). Özellikler her pakette aynı.
- Kartsız deneme standart: 7–30 gün.

### 7.5 Kapsam dışında kalmalı

- **Rota optimizasyonu:** Türkiye'de mobil kuaför sayısı bilinmiyor.
- **Pet oteli:** Karar zaten alınmış. PawBooking ve PetKay'in otel ürünü var; bu alanda rekabet etmeye gerek yok.
- **Yapay zekâ asistanı:** Rezzim ve Teddy'nin farkı, ama MVP için erken.
- **Perakende stok:** Dökümdeki ürün satırı şimdilik yeterli.
- **Tüketici pazaryeri:** PawBooking, Kolay Randevu ve Rezzim'in yaptığı bu. Faz 3'teki "pet sahibi uygulaması ve pazaryeri" için referans alınsın.

### 7.6 Açık sorular

- Pet kuaförlerle yapılacak keşif görüşmelerinde randevuların ne kadarının telefon ve WhatsApp'tan, ne kadarının Instagram DM'den geldiği sorulsun. Online randevu kararını bu belirler.
- PetKay'in ve Patipu'nun gerçek kullanıcı sayısı bilinmiyor. İki üründe demo hesap açılıp gerçek akış denenmeli.
- SalonAppy'nin web fiyatları doğrulanamadı. Bir tarayıcıdan tr.salonappy.com/fiyatlar kontrol edilmeli.

---

## 8. Kaynaklar

**Türkiye, pet'e özel**

- PetKay: [pet kuaför](https://petkay.com/pet-kuaforleri-rezervasyon-sistemi), [demo deneyimi](https://petkay.com/pet-kuafor-sistemi-deneyimi), [pet oteli](https://petkay.com/pet-oteli-rezervasyon-sistemi), [örnek işletme sayfası](https://petkay.com/petkuafor/miss-pati)
- Patipu: [ana sayfa ve fiyat](https://patipu.com/), [hakkında](https://patipu.com/about), [yol haritası](https://patipu.com/roadmap), [gelmeme oranı blog](https://patipu.com/blog/pet-kuaforlerinde-gelmeme-oranini-azaltma), [online randevu blog](https://patipu.com/blog/online-randevu-sayfasi-nasil-daha-cok-donusur), [KVKK](https://patipu.com/legal/kvkk-aydinlatma-metni)
- Lumiperi: [PetCareOS](https://lumiperi.com/tr-tr/pet-kuafor-veteriner-programi), [ana sayfa](https://lumiperi.com/tr-tr)
- PawBooking: [fiyat](https://business.pawbooking.co/pricing), [pet kuaför](https://business.pawbooking.co/cozumlerimiz/pet-kuafor-randevu-sistemi), [hatırlatma](https://business.pawbooking.co/cozumlerimiz/sms-hatirlatma-otomasyonu), [iptal/no-show](https://business.pawbooking.co/cozumlerimiz/iptal-no-show-politikasi), [widget](https://business.pawbooking.co/cozumlerimiz/online-rezervasyon-widget), [pazaryeri](https://pawbooking.co/), [Play Store](https://play.google.com/store/apps/details?id=com.pawbooking.partner)
- RandevuLS: [pet grooming](https://www.lsyazilim.com/randevuls/sektor/pet-grooming), [paketler](https://www.lsyazilim.com/randevuls/paketler)

**Türkiye, genel**

- Planla: [pet kuaför](https://planla.co/pet-kuaforu-randevu-programi), [fiyat](https://planla.co/pet-kuaforu-randevu-programi/fiyatlar), [App Store](https://apps.apple.com/tr/app/planla/id1659867306?l=tr)
- Salon Randevu: [pet kuaför](https://www.salonrandevu.app/pet-kuafor-randevu-uygulamasi), [paketler](https://www.salonrandevu.app/paket-liste.php), [Şikâyetvar](https://www.sikayetvar.com/salonrandevucom)
- AtaksAPP: [pet kuaför](https://www.ataks.app/pet-kuafor-programi/), [fiyat](https://www.ataks.app/fiyatlar/), [özellikler](https://www.ataks.app/ozellikler/)
- Marssoft: [ürünler](https://www.marssoft.com.tr/solutions), [fiyat](https://www.marssoft.com.tr/pricing)
- SalonAppy / Kolay Randevu: [App Store](https://apps.apple.com/tr/app/salonappy-salon-y%C3%B6netimi/id1097504667?l=tr), [Play Store](https://play.google.com/store/apps/details?id=com.kolayrandevu.isletme&hl=tr), [Şikâyetvar SalonAppy](https://www.sikayetvar.com/salonappy), [Şikâyetvar Kolay Randevu](https://www.sikayetvar.com/kolay-randevu)
- Rezzim: [salon programı](https://rezzim.com/salon-randevu-programi), [fiyat](https://rezzim.com/fiyatlandirma), [yerli program](https://rezzim.com/yerli-salon-programi)
- RandevuNet: [ana sayfa](https://randevunet.com/), [veteriner](https://randevunet.com/veteriner-randevu-programi)
- Kuaför Randevu: [ana sayfa](https://kuaforrandevu.com.tr/)

**Global**

- MoeGo: [fiyat](https://www.moego.pet/pricing?companyType=1), [grooming](https://www.moego.pet/pet-grooming-software), [rapor kartı](https://help.moego.pet/en/articles/11370580-custom-report-card-transform-your-pet-care-experience-with-personalized-pet-reports), [Capterra](https://www.capterra.com/p/165732/MoeGo/reviews/)
- DaySmart Pet: [fiyat](https://www.daysmart.com/pet/pricing/), [grooming](https://www.daysmart.com/pet/dog-grooming-software/), [yardım](https://help.daysmartpet.com/en/), [Capterra](https://www.capterra.com/p/93413/DaySmart-Pet/reviews/)
- Teddy: [fiyat](https://www.tryteddy.com/pricing), [hatırlatmalar](https://www.tryteddy.com/texting-reminders)
- GrooMore: [fiyat](https://www.groomore.com/pricing.html), [özellikler](https://www.groomore.com/all-features.html)
- Gingr: [fiyat](https://www.gingrapp.com/pricing), [PreCheck](https://www.gingrapp.com/precheck), [rapor kartı](https://www.gingrapp.com/blog/engage-customers-with-gingrs-report-cards), [Capterra](https://www.capterra.com/p/136469/Gingr/reviews/)
- PetExec: [groomers](https://www.petexec.net/service/groomers), [uyarılar](https://www.petexec.net/features/pet-and-owner-advisories), [Capterra](https://www.capterra.com/p/92864/PetExec/reviews/)
- Pawfinity: [fiyat](https://www.pawfinity.com/pricing/), [müşteri alacağı](https://www.pawfinity.com/features/client-account-credit-store-credit-gift-cards/)
- Animalo: [grooming](https://animalo.com/en/for_grooming)
