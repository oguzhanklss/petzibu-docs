# Petzibu – Rakip Analizi

Sep 23, 2026 · @OGZ

## Özet

Pazar ikiye ayrılıyor. Petzibu bu iki grubun arasındaki boşluğa oturuyor. Global araçların pet veri modeli derin, ama hiçbirinde Türkçe, WhatsApp ya da KVKK desteği yok. Türk araçlarında WhatsApp, adisyon ve borç takibi var, ama gerçek bir pet veri modeli yok.

12 rakibi inceledim: 8 global, 4 Türk. PetSmart bir yazılım değil, bir kuaför zinciri. Bu yüzden matrise almadım.

**En önemli 6 bulgu**

1. **Online randevu 12 rakibin 12'sinde de var.** Petzibu'da ise Faz 3'te. Kapsamdaki en büyük fark bu.
2. **Pet veri modelini Türkiye'de düzgün kuran yok.** Salon Randevu, RandevuNet ve SalonAppy'de hayvan ayrı bir kayıt değil. Sitelerinde aşı süresi, boyut kademesi ve uyarı etiketi de görünmüyor. Pet'e özel tek ciddi yerli ürün RandevuLS: yıllık 15.000 ₺'den başlıyor ve bir web şablonu.
3. **Borç ve gider takibi Türk araçlarında standart, global araçlarda zayıf.** Petzibu'nun E7 epic'i bu açıdan doğru yerde.
4. **İki şeyi kimse net yapmıyor:** ödeme anında sonraki randevu önerisi ve bakım raporunu WhatsApp'tan gönderme. İkisi de Petzibu MVP'sinde var, dolayısıyla farklılaşma fırsatı.
5. **Paket/seans kartı 10 rakipte, stok 9 rakipte var.** Petzibu'nun hiçbir fazında ikisi de yok.
6. **Türkiye'de fiyat baskısı yüksek.** RandevuNet ücretsiz. Salon Randevu'nun ve SalonAppy'nin ücretsiz katmanı var. Bu durumda ücret ödenecek fark, pet'e özel özellikler olmalı.

## Rakipler

Grooming'e odaklanan global ürünler MoeGo, Teddy ve GrooMore. Gingr, PetExec ve Animalo'nun asıl işi pansiyon ve kreş; grooming onlarda yan modül. Türk araçlarının üçü genel salon yazılımı, biri pet'e özel bir şablon.

| Rakip                                                                       | Konumlanma                                                  | Hedef                                        | Platform                          | Pazar                   | Öne çıkan                                                        | Zayıf yanı                                                |
| --------------------------------------------------------------------------- | ----------------------------------------------------------- | -------------------------------------------- | --------------------------------- | ----------------------- | ---------------------------------------------------------------- | --------------------------------------------------------- |
| [MoeGo](https://www.moego.pet/)                                             | Grooming öncelikli herşey-bir-arada                         | Orta ve büyük salon, mobil araç filosu       | Web, iOS, Android                 | ABD; 10 bin+ işletme    | Smart Schedule, fotoğraflı rapor kartı, üyelik                   | Pahalı kademeler, SMS kotası, Android hataları            |
| [DaySmart Pet](https://www.daysmart.com/pet/)                               | Eski 123Pet; köklü grooming yazılımı                        | Tek kişilikten zincire                       | Web, masaüstü, iOS, Android       | ABD; 5 bin+ kuaför      | En düşük giriş fiyatı, borç takibi, bekleme listesi              | Eski arayüz, telefon desteği kaldırıldı, yıllık zam       |
| [Teddy](https://www.tryteddy.com/)                                          | Yapay zekâ asistanlı grooming yazılımı                      | Tek kişilik ve küçük salon                   | Web; SMS ve ses komutu            | ABD; 1 bin+ groomer     | Sınırsız SMS, "Ask Teddy" asistanı, AI resepsiyonist             | Aşı ve rapor kartı sitede geçmiyor, ödeme yalnızca Square |
| [GrooMore](https://www.groomore.com/)                                       | Uygun fiyatlı, kolay grooming yazılımı                      | Salon ve mobil araç                          | Web, iOS, Android                 | ABD                     | Neredeyse tüm özellikler en ucuz pakette                         | iPhone'da personel kısıtı, pansiyon yok                   |
| [Gingr](https://www.gingrapp.com/)                                          | Pansiyon ve kreş platformu, grooming modülü                 | Çok hizmetli tesis, zincir                   | Web; markalı müşteri uygulaması   | ABD; 5 bin+ tesis       | PreCheck, aşısı geçmiş hayvana randevuyu engelleme               | Personel uygulaması 1,6★, kurulum uzun                    |
| [PetExec](https://www.petexec.net/)                                         | Pansiyon ve kreş; 2024'ten beri Gingr ile aynı çatı altında | Çok hizmetli tesis                           | Web, iOS, Android                 | ABD, Kanada, Avustralya | $19,99'a sınırsız SMS, takvimde uyarı ikonları                   | Grooming'e özel akışlar zayıf                             |
| [Pawfinity](https://www.pawfinity.com/)                                     | Sabit fiyatlı herşey-bir-arada                              | Salon, mobil araç, pansiyon                  | Web ve PWA (yerel uygulama değil) | ABD, Kanada             | Sınırsız personel ve fotoğraf, Booking Genius rota önerisi       | SMS ayrıca ücretli, telefonda ekran sorunları             |
| [Animalo](https://animalo.com/)                                             | Avrupa'da çok dilli, ucuz platform                          | Asıl pansiyon; grooming için ayrı ucuz paket | Web                               | Fransa merkezli; 7 dil  | €9,99'luk grooming paketi, ülkeye göre yerelleştirme             | SMS Haziran 2026'da geldi, POS ve stok yok                |
| [Salon Randevu](https://www.salonrandevu.app/pet-kuafor-randevu-uygulamasi) | Genel salon yazılımı, pet kuaför sayfası var                | Küçük salon, şube                            | Web, iOS, Android, Huawei         | Türkiye                 | WhatsApp eklentisi, adisyon, borç, e-imza                        | Pet veri modeli yok, ani fiyat artışları                  |
| [RandevuNet](https://randevunet.com/)                                       | "Ömür boyu ücretsiz" randevu yazılımı                       | Tek kişilikten çok şubeye                    | Web, iOS, Android                 | Türkiye; 810+ şube      | Önce WhatsApp, iletilmezse SMS; sesli onay araması; Instagram DM | Pet desteği yalnızca veteriner notları                    |
| [RandevuLS](https://www.lsyazilim.com/randevuls/sektor/pet-grooming)        | 8 sektöre satılan şablon ürün, pet sayfası var              | Tek şubeden kurumsala                        | Web; kiosk tablet                 | Türkiye; 4 dil          | Irka göre süre, aşı ve huy notu, dijital onam formu              | Yıllık peşin ödeme, yerel işletme uygulaması yok          |
| [Kolay Randevu / SalonAppy](https://www.kolayrandevu.com/)                  | Güzellik salonu yazılımı ve tüketici pazaryeri              | Küçük salon, şube                            | Web, iOS, Android                 | Türkiye; 1 bin+ işletme | Kendi pazaryerinden müşteri getiriyor, 4,8★ (yaklaşık 3.400 oy)  | Pet hedeflemiyor, veri erişimi kesilen hesaplar şikâyeti  |

**Ayrıca bakılması gereken:** [PawBooking Business](https://business.pawbooking.co/hizmetler/pet-kuafor-randevu-programi). Pet kuaför için yapılmış yerli bir ürün ve 1.200 ₺/ay'dan başlıyor. Büyük olasılıkla Petzibu'nun en yakın doğrudan rakibi. Bu turda derinlemesine incelemedim.

## Özellik matrisi

44 özellik, 12 rakip. Petzibu sütunu, özelliğin epics.md'de hangi epic'e ya da faza düştüğünü gösteriyor.

**Semboller:** ✓ var · ◐ kısmi · ✗ yok · ? resmî kaynakta doğrulanamadı.

"?" çoğu zaman "sitede anlatılmıyor" anlamına geliyor, "yok" anlamına değil. En çok "?" Türk araçlarında çıkıyor, çünkü sitelerinde özellikleri ayrıntılı anlatmıyorlar.

### Kurulum, müşteri ve hayvan

| Özellik                             | Petzibu                  | MoeGo | DaySmart | Teddy | GrooMore | Gingr | PetExec | Pawfinity | Animalo | Salon R. | RandevuNet | RandevuLS | Kolay R. |
| ----------------------------------- | ------------------------ | ----- | -------- | ----- | -------- | ----- | ------- | --------- | ------- | -------- | ---------- | --------- | -------- |
| İşletme mobil uygulaması            | MVP                      | ✓     | ✓        | ◐     | ✓        | ◐     | ✓       | ◐         | ◐       | ✓        | ✓          | ◐         | ✓        |
| Web paneli                          | MVP                      | ✓     | ✓        | ✓     | ✓        | ✓     | ✓       | ✓         | ✓       | ✓        | ✓          | ✓         | ✓        |
| Boy, ırk ya da tüye göre fiyat      | MVP · E1 (yalnız boy)    | ✓     | ?        | ?     | ✓        | ◐     | ◐       | ✓         | ✓       | ?        | ?          | ◐         | ?        |
| Ek ücret kalemleri                  | MVP · E1                 | ✓     | ◐        | ✓     | ?        | ✓     | ◐       | ✓         | ◐       | ?        | ?          | ?         | ?        |
| Müşteri başına birden fazla hayvan  | MVP · E2                 | ✓     | ✓        | ◐     | ✓        | ✓     | ✓       | ✓         | ✓       | ◐        | ◐          | ◐         | ◐        |
| Uyarı etiketinin takvimde görünmesi | MVP · E2, E4             | ◐     | ◐        | ◐     | ◐        | ?     | ✓       | ◐         | ◐       | ?        | ?          | ◐         | ?        |
| Aşı takibi ve süre uyarısı          | MVP · E2                 | ✓     | ✓        | ?     | ✓        | ✓     | ✓       | ◐         | ✓       | ?        | ◐          | ◐         | ?        |
| Fotoğraf geçmişi                    | MVP · E2                 | ◐     | ✓        | ◐     | ?        | ✓     | ◐       | ✓         | ◐       | ?        | ?          | ?         | ?        |
| Dijital kayıt formu                 | MVP · E3                 | ✓     | ✓        | ✓     | ✓        | ✓     | ✓       | ✓         | ✓       | ?        | ?          | ✓         | ?        |
| Sözleşme / e-imza                   | Yok                      | ✓     | ✓        | ✓     | ✓        | ✓     | ✓       | ✓         | ◐       | ✓        | ?          | ✓         | ?        |
| Başka yazılımdan veri aktarma       | MVP rehber · Faz 2 Excel | ✓     | ✓        | ✓     | ?        | ✓     | ✓       | ✓         | ?       | ?        | ✓          | ✓         | ?        |

### Takvim ve randevu

| Özellik                              | Petzibu                      | MoeGo | DaySmart | Teddy | GrooMore | Gingr | PetExec | Pawfinity | Animalo | Salon R. | RandevuNet | RandevuLS | Kolay R. |
| ------------------------------------ | ---------------------------- | ----- | -------- | ----- | -------- | ----- | ------- | --------- | ------- | -------- | ---------- | --------- | -------- |
| Gün ve hafta görünümü, sürükle-bırak | MVP · E4 (sürükle-bırak yok) | ✓     | ✓        | ✓     | ◐        | ✓     | ✓       | ◐         | ◐       | ✓        | ✓          | ?         | ✓        |
| Çok hayvanlı randevu                 | MVP · E4                     | ✓     | ✓        | ?     | ?        | ✓     | ?       | ◐         | ?       | ?        | ◐          | ?         | ?        |
| Tekrarlayan randevu                  | Yok (K7)                     | ✓     | ✓        | ?     | ✓        | ✓     | ◐       | ✓         | ?       | ?        | ?          | ?         | ?        |
| Müşterinin online randevu alması     | Faz 3                        | ✓     | ✓        | ✓     | ✓        | ✓     | ✓       | ✓         | ✓       | ✓        | ✓          | ✓         | ✓        |
| Bekleme listesi                      | Faz 3                        | ✓     | ✓        | ✓     | ✓        | ✓     | ◐       | ◐         | ?       | ?        | ?          | ?         | ?        |
| Check-in ve durum akışı              | MVP · E5                     | ✓     | ◐        | ✓     | ✓        | ✓     | ✓       | ?         | ◐       | ?        | ✓          | ?         | ?        |
| Fotoğraflı bakım raporu              | MVP · E5                     | ✓     | ?        | ?     | ?        | ✓     | ◐       | ✓         | ✓       | ?        | ?          | ?         | ?        |
| Ödemede sonraki randevu önerisi      | MVP · E5                     | ◐     | ?        | ?     | ?        | ?     | ?       | ?         | ?       | ?        | ?          | ?         | ?        |

### İletişim ve pazarlama

| Özellik                             | Petzibu                   | MoeGo | DaySmart | Teddy | GrooMore | Gingr | PetExec | Pawfinity | Animalo | Salon R. | RandevuNet | RandevuLS | Kolay R. |
| ----------------------------------- | ------------------------- | ----- | -------- | ----- | -------- | ----- | ------- | --------- | ------- | -------- | ---------- | --------- | -------- |
| Otomatik randevu hatırlatması       | MVP · E6 (yarı otomatik)  | ✓     | ✓        | ✓     | ✓        | ✓     | ✓       | ✓         | ✓       | ✓        | ✓          | ✓         | ✓        |
| WhatsApp ile müşteriye mesaj        | MVP elle · Faz 2 otomatik | ?     | ?        | ?     | ?        | ?     | ?       | ?         | ?       | ✓        | ✓          | ✓         | ?        |
| İki yönlü mesajlaşma (uygulama içi) | Yok (WhatsApp'ta)         | ✓     | ✓        | ✓     | ✓        | ◐     | ✓       | ✓         | ◐       | ?        | ◐          | ?         | ?        |
| Rebook ve aşı hatırlatması          | MVP · E6                  | ◐     | ◐        | ◐     | ◐        | ◐     | ◐       | ?         | ✓       | ?        | ◐          | ?         | ?        |
| Pazarlama kampanyaları              | Faz 3                     | ✓     | ✓        | ◐     | ✓        | ✓     | ✓       | ✓         | ◐       | ◐        | ?          | ?         | ?        |
| Yorum toplama (Google)              | Yok                       | ✓     | ✓        | ✓     | ✓        | ✓     | ◐       | ✓         | ?       | ?        | ✓          | ?         | ✓        |
| Müşteri uygulaması veya portalı     | Faz 3                     | ◐     | ◐        | ?     | ◐        | ✓     | ✓       | ◐         | ✓       | ✓        | ✓          | ✓         | ✓        |
| Yapay zekâ                          | Yok                       | ◐     | ?        | ✓     | ✗        | ?     | ?       | ?         | ?       | ✓        | ◐          | ?         | ?        |

### Para

| Özellik                                | Petzibu                     | MoeGo | DaySmart | Teddy | GrooMore | Gingr | PetExec | Pawfinity | Animalo | Salon R. | RandevuNet | RandevuLS | Kolay R. |
| -------------------------------------- | --------------------------- | ----- | -------- | ----- | -------- | ----- | ------- | --------- | ------- | -------- | ---------- | --------- | -------- |
| Entegre ödeme / POS                    | Yok (elle kayıt)            | ✓     | ✓        | ✓     | ✓        | ✓     | ✓       | ✓         | ◐       | ?        | ◐          | ?         | ◐        |
| Depozito, kayıtlı kart, no-show ücreti | Faz 3                       | ✓     | ✓        | ◐     | ✓        | ◐     | ◐       | ◐         | ✓       | ?        | ◐          | ?         | ?        |
| Hizmet dökümü / fiş / adisyon          | MVP · E7                    | ✓     | ✓        | ✓     | ◐        | ✓     | ✓       | ✓         | ✓       | ✓        | ?          | ?         | ◐        |
| Müşteri borcu / veresiye               | MVP · E7                    | ?     | ✓        | ?     | ?        | ✓     | ✓       | ◐         | ?       | ✓        | ✓          | ?         | ✓        |
| Gider kaydı                            | MVP · E7                    | ?     | ◐        | ?     | ?        | ?     | ?       | ✓         | ?       | ✓        | ✓          | ?         | ✓        |
| Raporlama                              | MVP · E7 özet · Faz 2 detay | ✓     | ✓        | ✓     | ✓        | ✓     | ✓       | ◐         | ✓       | ✓        | ✓          | ◐         | ✓        |
| Paket / seans / üyelik                 | Yok                         | ✓     | ✓        | ?     | ?        | ✓     | ✓       | ✓         | ✓       | ✓        | ✓          | ✓         | ✓        |
| Hediye kartı / sadakat puanı           | Yok                         | ◐     | ✓        | ?     | ✓        | ◐     | ◐       | ◐         | ◐       | ✓        | ?          | ◐         | ✓        |
| Perakende stok                         | Yok                         | ✓     | ✓        | ◐     | ✓        | ✓     | ✓       | ✓         | ?       | ✓        | ✓          | ?         | ✓        |

### Personel, ölçek ve uyum

| Özellik                          | Petzibu                     | MoeGo | DaySmart | Teddy | GrooMore | Gingr | PetExec | Pawfinity | Animalo             | Salon R. | RandevuNet | RandevuLS | Kolay R. |
| -------------------------------- | --------------------------- | ----- | -------- | ----- | -------- | ----- | ------- | --------- | ------------------- | -------- | ---------- | --------- | -------- |
| Personel takvimi, rol ve yetki   | Faz 2                       | ✓     | ✓        | ✓     | ✓        | ✓     | ◐       | ✓         | ◐                   | ✓        | ✓          | ✓         | ✓        |
| Prim / komisyon / bahşiş         | Faz 2                       | ✓     | ✓        | ✓     | ✓        | ✓     | ✓       | ◐         | ?                   | ✓        | ◐          | ✓         | ✓        |
| Bordro / mesai kaydı             | Faz 2                       | ✓     | ✓        | ◐     | ?        | ◐     | ◐       | ✓         | ?                   | ?        | ?          | ?         | ?        |
| Mobil araç için rota             | Yok                         | ✓     | ✓        | ✓     | ✓        | ?     | ?       | ✓         | ?                   | ?        | ?          | ?         | ?        |
| Çoklu şube                       | Yok (veri modeli hazır, K5) | ✓     | ✓        | ✓     | ✓        | ✓     | ✓       | ◐         | ✓                   | ✓        | ✓          | ✓         | ✓        |
| Pansiyon / kreş                  | Yok                         | ✓     | ✓        | ✗     | ✗        | ✓     | ✓       | ✓         | ✓                   | ?        | ?          | ◐         | ?        |
| Veri dışa aktarma / entegrasyon  | MVP · E8 dışa aktarma       | ✓     | ✓        | ◐     | ✓        | ✓     | ✓       | ◐         | ◐                   | ?        | ✓          | ◐         | ◐        |
| Yerel uyum (KVKK/GDPR, e-fatura) | MVP · KVKK (E3, E8)         | ✗     | ✗        | ✗     | ✗        | ?     | ?       | ◐ GDPR    | ◐ GDPR, FR e-fatura | ?        | ◐ KVKK     | ◐ KVKK    | ?        |

## Fiyatlandırma

Global araçların giriş fiyatı ayda $29 ile $109 arasında. Türk araçlarının çoğu ise ücretsiz başlıyor. Mesaj maliyeti her yerde ayrı bir kalem ve rakipler arasında en çok şikâyet edilen konu.

Fiyatlar satıcının kendi para biriminde ve aylık. TL'ye çevirmedim.

| Rakip                                                                                                      | Giriş paketi                          | En üst paket                                                    | Mesaj modeli                                                    | Deneme / ücretsiz                      |
| ---------------------------------------------------------------------------------------------------------- | ------------------------------------- | --------------------------------------------------------------- | --------------------------------------------------------------- | -------------------------------------- |
| [MoeGo](https://www.moego.pet/pricing)                                                                     | $79 salon, $49 mobil                  | $239; Enterprise teklifle                                       | Kota: 300–1.350 SMS                                             | Belirtilmemiş                          |
| [DaySmart Pet](https://www.daysmart.com/pet/pricing/)                                                      | $29                                   | $199; Premium Growth teklifle                                   | Kota: 500–5.000 SMS; iki yönlü mesaj $149'luk paketten itibaren | 14 gün                                 |
| [Teddy](https://www.tryteddy.com/pricing)                                                                  | $49 (1 groomer)                       | $199                                                            | Sınırsız iki yönlü SMS                                          | 14 gün                                 |
| [GrooMore](https://www.groomore.com/pricing.html)                                                          | $49                                   | $79; çok şube teklifle                                          | Kota: 400–800 SMS                                               | "Ücretsiz başla" (kaynaklar çelişiyor) |
| [Gingr](https://www.gingrapp.com/pricing)                                                                  | Spa (grooming) paketi $109            | Stay $209; Enterprise teklifle                                  | İki yönlü SMS ücretli eklenti                                   | Belirtilmemiş                          |
| [PetExec](https://www.petexec.net/purchase-petexec)                                                        | Yaklaşık $105 (üçüncü taraf)          | Tek paket                                                       | Sınırsız SMS eklentisi $19,99                                   | 30 gün para iadesi                     |
| [Pawfinity](https://www.pawfinity.com/pricing/)                                                            | $60 (yıllıkta $55)                    | $110                                                            | SMS başına $0,05                                                | 7 gün                                  |
| [Animalo](https://animalo.com/en/for_grooming)                                                             | Grooming Solo €9,99                   | Grooming Salon €19,99                                           | Ön ödemeli SMS kredisi                                          | 30 gün                                 |
| [Salon Randevu](https://www.salonrandevu.app/paket-liste.php)                                              | Silver ücretsiz (3 personel, 100 SMS) | Gold 599 ₺ + KDV, Platinum 949 ₺ + KDV; Diamond teklifle        | SMS paketi (500 SMS 115 ₺); WhatsApp eklentisi 79 ₺/ay          | Ücretsiz katman                        |
| [RandevuNet](https://randevunet.com/)                                                                      | Basit ücretsiz (reklamlı)             | Profesyonel 417 ₺/ay veya 4.999 ₺/yıl                           | Ücretsizde kredi, Pro'da sınırsız                               | Ücretsiz katman                        |
| [RandevuLS](https://www.lsyazilim.com/randevuls/paketler)                                                  | 15.000 ₺/yıl + KDV                    | 30.000 ₺/yıl + KDV                                              | NetGSM üzerinden, fiyatı açıklanmamış                           | Demo                                   |
| [Kolay Randevu / SalonAppy](https://apps.apple.com/tr/app/salonappy-salon-y%C3%B6netimi/id1097504667?l=tr) | Ayda 100 randevuya kadar ücretsiz     | Uygulama içi satın alma 1.499–3.249,99 ₺ (dönemi belirtilmemiş) | Bilinmiyor                                                      | Ücretsiz katman                        |

Salon Randevu'nun fiyatı sayfadan sayfaya değişiyor. Tabloda paket listesindeki rakamlar var, ama pet kuaför sayfasında ve App Store'da farklı fiyatlar yazıyor. Şikâyetvar'da da yıllık 2.800 ₺'den 12.000 ₺'ye zam yapıldığına dair şikâyet var.

## Petzibu kapsamına etkisi

MVP kapsamı rakiplere göre doğru yerde. Düşük maliyetli 4 ekleme var. Faz sırasında da tartışılması gereken 2 konu var.

### Rakiplere göre doğrulanan MVP kararları

- **Çok hayvanlı randevu (K1):** MoeGo, DaySmart ve Gingr'da var. Türk araçlarının hiçbirinde yok.
- **Boyut kademesi, uyarı etiketi ve aşı durumu (E1, E2):** Global araçlarda standart. Türkiye'de ise yalnızca RandevuLS'te kısmen var.
- **Borç ve gider (E7):** Türk araçlarında standart. MoeGo'da ve Teddy'de bulunamadı. Türk kuaförü bunu bekleyecek.
- **WhatsApp öncelikli iletişim (K3):** Global rakiplerin hiçbirinde WhatsApp yok. Yerelde Salon Randevu bunu ücretli eklenti olarak satıyor, RandevuNet ise ücretsiz veriyor.
- **Personeli Faz 2'ye bırakmak:** Teddy $49'luk paketi tek groomer için satıyor. Tek kişilik salonun gerçek bir segment olduğunu gösteriyor.

### MVP için düşük maliyetli eklemeler

| Ekleme                                                             | Hangi epic | Neden                                                                                             | Rakiplerde                           |
| ------------------------------------------------------------------ | ---------- | ------------------------------------------------------------------------------------------------- | ------------------------------------ |
| Bakım riski onayı: keçe, kısa tıraş, yaşlı hayvan için onay kutusu | E3         | Form builder gerekmez, K6 ile uyumlu. Tartışmalı durumda salonu korur.                            | 10 rakipte sözleşme ya da e-imza var |
| Takvimde sürükle-bırak ile erteleme                                | E4         | Telefonda süren bir görüşmede tek dokunuşla erteleme                                              | 8 rakipte var                        |
| Tamamlanma mesajına Google yorum linki                             | E5, E6     | Mesaj şablonuna eklenen bir değişken kadar iş                                                     | 8 rakipte yorum toplama var          |
| Aşısı geçmiş hayvana randevu açarken güçlü uyarı                   | E4         | Gingr bu randevuyu tamamen engelliyor. Petzibu'nun "uyar ama engelleme" ilkesi burada da geçerli. | Gingr, MoeGo                         |

### Faz sırası için tartışılacaklar

1. **Online randevu Faz 3'ten Faz 2'ye çekilmeli mi?** 12 rakibin hepsinde var. Ucuz bir yolu da var: Teddy ve Gingr gibi "talep gönder, salon onaylasın" modeli. Bu model E3'teki onay kuyruğunu yeniden kullanır ve slot bazlı müsaitlik gerektirmez.
2. **Paket ve seans kartı (örn. 5 yıkama al, 1 bedava) hangi faza girmeli?** 10 rakipte var ve Türk salonlarında alışkanlık haline gelmiş. Şu an hiçbir fazda yok. E7'deki borç bakiyesinin tersi olan "ön ödemeli bakiye" olarak Faz 2'ye girebilir.

### Farklılaşma fırsatları

- **Pet veri modeli ve Türkiye'ye özgü temel finans bir arada.** İkisini birlikte sunan bir rakip yok.
- **Sonraki randevu önerisi (E5):** Ödeme anında sonraki randevuyu öneren tek rakip MoeGo, o da kısmen. Rebook oranı doğrudan ciroya dönüşüyor.
- **Telefonla hızlı randevu:** Tasarımdaki "Tek elle hızlı randevu girişi" ekranının bir benzerini hiçbir rakipte görmedim.
- **Şeffaf ve sabit fiyat:** Rakiplerle ilgili en sık şikâyetler SMS kotası, ani zamlar ve üst pakete kilitlenen özellikler.

### Kapsam dışında kalmalı

- **Rota optimizasyonu:** Türkiye'de mobil araçla kuaförlük yapan işletme sayısı dahil henüz bilinmiyor.
- **Pansiyon ve kreş:** Karar zaten alınmış.
- **Yapay zekâ asistanı:** Teddy'nin farkı bu, ama MVP için erken.
- **Perakende stok:** Kuaförle yapılacak keşif görüşmelerinde stok ihtiyacı çıkarsa Faz 2'de ele alınabilir.

## Kaynaklar

Özellik ve fiyat bilgileri satıcıların kendi sitelerinden, yardım merkezlerinden ve uygulama mağazası sayfalarından alındı. Şikâyetler Capterra, Trustpilot ve Şikâyetvar'dan geldi. Rakip bloglarını (Teddy, GroomBoard, Animalo) yalnızca rakip listesini çıkarmak için kullandım.

- MoeGo: [fiyat](https://www.moego.pet/pricing?companyType=1), [grooming](https://www.moego.pet/pet-grooming-software), [aşı](https://www.moego.pet/help/en/articles/12845387-pet-vaccine), [rapor kartı](https://help.moego.pet/en/articles/11370580-custom-report-card-transform-your-pet-care-experience-with-personalized-pet-reports), [Capterra](https://www.capterra.com/p/165732/MoeGo/reviews/)
- DaySmart Pet: [fiyat](https://www.daysmart.com/pet/pricing/), [grooming](https://www.daysmart.com/pet/dog-grooming-software/), [yardım merkezi](https://help.daysmartpet.com/en/), [Capterra](https://www.capterra.com/p/93413/DaySmart-Pet/reviews/)
- Teddy: [fiyat](https://www.tryteddy.com/pricing), [SSS](https://tryteddy.com/faqs), [AI resepsiyonist](https://tryteddy.com/ai-receptionist)
- GrooMore: [fiyat](https://www.groomore.com/pricing.html), [tüm özellikler](https://www.groomore.com/all-features.html), [App Store](https://apps.apple.com/us/app/groomore-grooming-software/id1541590519)
- Gingr: [fiyat](https://www.gingrapp.com/pricing), [grooming](https://www.gingrapp.com/pet-grooming-software), [PreCheck](https://www.gingrapp.com/precheck), [Capterra](https://www.capterra.com/p/136469/Gingr/reviews/)
- PetExec: [groomers](https://www.petexec.net/service/groomers), [tüm özellikler](https://www.petexec.net/all-features), [sınırsız SMS](https://www.petexec.net/unlimited-texting), [Capterra](https://www.capterra.com/p/92864/PetExec/reviews/)
- Pawfinity: [fiyat](https://www.pawfinity.com/pricing/), [özellikler](https://www.pawfinity.com/features/), [Capterra](https://www.capterra.com/p/142461/Pawfinity/reviews/)
- Animalo: [grooming](https://animalo.com/en/for_grooming), [ana sayfa](https://animalo.com/), [sürüm notları](https://app.animalo.com/whats-new)
- Salon Randevu: [pet kuaför sayfası](https://www.salonrandevu.app/pet-kuafor-randevu-uygulamasi), [paketler](https://www.salonrandevu.app/paket-liste.php), [Şikâyetvar](https://www.sikayetvar.com/salonrandevucom)
- RandevuNet: [ana sayfa](https://randevunet.com/), [veteriner sayfası](https://randevunet.com/veteriner-randevu-programi)
- RandevuLS: [pet grooming](https://www.lsyazilim.com/randevuls/sektor/pet-grooming), [paketler](https://www.lsyazilim.com/randevuls/paketler)
- Kolay Randevu / SalonAppy: [kolayrandevu.com](https://www.kolayrandevu.com), [App Store](https://apps.apple.com/tr/app/salonappy-salon-y%C3%B6netimi/id1097504667?l=tr), [Şikâyetvar](https://www.sikayetvar.com/salonappy)
- PawBooking Business: [pet kuaför programı](https://business.pawbooking.co/hizmetler/pet-kuafor-randevu-programi)
