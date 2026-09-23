# Petzibu – Epic Haritası (MVP)

> Kapsam: Yalnızca **salon sahibi** (owner). Pet sahibi uygulaması yok.
> Tek müşteriye dönük yüzey: **Intake Form** linki (hesapsız).
> Durum: Taslak v1 · 23 Eylül 2026

Her epic kendi dosyasında yaşar. Kapsam maddeleri checkbox'tır; bir madde bitince işaretlenir ve aşağıdaki tabloda durum güncellenir. İlerleme tek yerden, bu tablodan takip edilir.

## Epic'ler

| #   | Epic                                           | Durum                       | Platform                             | Bağımlılık |
| --- | ---------------------------------------------- | --------------------------- | ------------------------------------ | ---------- |
| E1  | [Kurulum & İşletme](01-kurulum-isletme.md)     | Yapılacak (story'ler hazır) | Mobil öncelikli                      | —          |
| E2  | [Müşteri & Hayvan](02-musteri-hayvan.md)       | Yapılacak (story'ler hazır) | Mobil + Web                          | E1         |
| E3  | [Intake Form](03-intake-form.md)               | Yapılacak (story'ler hazır) | Müşteri tarafı web; onay Mobil + Web | E2, E8, E5 |
| E4  | [Takvim & Randevu](04-takvim-randevu.md)       | Yapılacak (story'ler hazır) | Mobil + Web                          | E1, E2     |
| E5  | [Randevu Operasyonu](05-randevu-operasyonu.md) | Yapılacak (story'ler hazır) | Mobil öncelikli                      | E4         |
| E6  | [Hatırlatmalar](06-hatirlatmalar.md)           | Yapılacak                   | Mobil öncelikli                      | E4, E5     |
| E7  | [Tahsilat & Kasa](07-tahsilat-kasa.md)         | Yapılacak (story'ler hazır) | Mobil + Web                          | E5         |
| E8  | [Hesap & Veri](08-hesap-veri.md)               | Yapılacak                   | Mobil + Web                          | E1         |

Durum değerleri: `Yapılacak` → `Devam ediyor` → `Tamamlandı`

## Story yazım sırası

**E1 → E2 → E4 → E5 → E7 → E6 → E3 → E8**

Sıranın mantığı veri bağımlılığı: önce hizmet ve kademe tanımı (E1), sonra müşteri ve hayvan modeli (E2), sonra randevu (E4, E5), sonra para (E7). Hatırlatmalar ve intake form bu modellerin üzerine kurulur.

---

## Platform stratejisi

- **Mobil**, günlük operasyonun tamamını kapsar. Salonda, tek elle, köpek masadayken yapılan her iş mobilde eksiksiz olmalı.
- **Web**, mobilin kopyası değil. Büyük ekranın gerçekten fark yarattığı masa başı işler için bir back office: hafta görünümü, raporlar, import/export.
- Story'ler platformdan bağımsız yazılır. Hangi platformda olacağı her story'de **Platform** alanıyla belirtilir.

| Etiket          | Anlamı                                 |
| --------------- | -------------------------------------- |
| Mobil           | Yalnızca mobilde                       |
| Mobil + Web     | Her iki platformda                     |
| Mobil öncelikli | Mobilde tam, web'de sonra veya sınırlı |

---

## Alınan kararlar

| #   | Karar                                                                                                                                                                                                                                                                                                                                         | Etkisi                                                                                                                                                                     |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| K1  | Bir randevu içinde **birden fazla hayvan** olabilir (MoeGo modeli). Randevu müşteri ve saat bilgisini tutar, altında hayvan başına hizmet satırları bulunur.                                                                                                                                                                                  | Tasarım 21 tek blok olarak güncellenir. Hatırlatma, check-in ve döküm randevu başına bir tane olur.                                                                        |
| K2  | "Fatura", müşteriye gönderilen **hizmet dökümü ve tahsilat kaydı** demek. Yasal fatura değildir.                                                                                                                                                                                                                                              | E-arşiv MVP dışında. Gelir, tamamlama anında değil **tahsilat** anında oluşur.                                                                                             |
| K3  | Hatırlatmalar MVP'de **yarı otomatik**: uygulama bildirim gönderir, owner tek dokunuşla hazır WhatsApp mesajını açar.                                                                                                                                                                                                                         | Otomatik WhatsApp Faz 2'de gelir, aynı mesaj şablonlarını kullanır.                                                                                                        |
| K4  | Hizmetlerde **boyut kademesi** var: küçük / orta / büyük için ayrı süre ve fiyat.                                                                                                                                                                                                                                                             | Hayvanın bir boyut alanı olur. Randevu fiyatı kademeden otomatik gelir.                                                                                                    |
| K5  | Tenant **salon sahibi değil, işletmedir**. Owner, o işletmenin bir kullanıcısıdır.                                                                                                                                                                                                                                                            | Faz 2'deki personel modülü için veri modeli baştan hazır olur.                                                                                                             |
| K6  | ~~Intake Form, sabit bir şablondur. Owner bazı alanları açıp kapatabilir. Form builder yoktur.~~                                                                                                                                                                                                                                              | K24 ile değiştirildi: alan açma/kapama da yok.                                                                                                                             |
| K7  | Tekrarlayan randevu MVP'de yok. Bu ihtiyacı sonraki randevu akışı ve rebook hatırlatması karşılar.                                                                                                                                                                                                                                            |                                                                                                                                                                            |
| K8  | Giriş **e-posta + şifre** ile yapılır. SMS OTP, Google ile giriş ve Apple ile giriş MVP'de yok.                                                                                                                                                                                                                                               | E1 giriş akışı sadeleşir; SMS sağlayıcı gerekmez.                                                                                                                          |
| K9  | Uygulamada **kayıt ekranı yok**. İşletme ve owner hesabını Petzibu ekibi açar; owner davet linkiyle şifresini belirler.                                                                                                                                                                                                                       | Kayıt akışı yerine davet akışı (ADM-01, KUR-01).                                                                                                                           |
| K10 | Ödeme **uygulama dışında** alınır. Uygulamada fiyat, abonelik ekranı veya ödemeye yönlendiren bir ifade bulunmaz (App Store 3.1.3(f)).                                                                                                                                                                                                        | Ödeme bekliyor ve Askıda uyarıları mobilde yalnızca "Petzibu ile iletişime geç" der; web'de "Ödeme bilgileri" IBAN/iletişim sayfasını açar (K34).                          |
| K11 | Pilot → ücretli geçişte hesap ve veri aynı kalır; yalnızca işletmenin durumu değişir.                                                                                                                                                                                                                                                         | İşletme durumu: Pilot / Aktif / Ödeme bekliyor / Askıda (ADM-02, K34).                                                                                                     |
| K12 | Boyut kademesi kilo eşikleri sabit: küçük < 10 kg, orta 10–25 kg, büyük > 25 kg.                                                                                                                                                                                                                                                              | İşletmeye özel eşik yok. K4'ü tamamlar.                                                                                                                                    |
| K13 | Çalışma saatlerinde öğle arası yok. Özel kapalı günler MVP'de var.                                                                                                                                                                                                                                                                            | Gün başına tek aralık (KUR-06); kapalı günler ayrı story (KUR-09).                                                                                                         |
| K14 | Davet ve şifre sıfırlama linkleri **her zaman web'de** açılır. MVP'de tek amaçlı sayfalardan oluşan küçük bir public web yüzeyi olur: davet kabulü, şifre sıfırlama, ileride intake formu (E3). Owner back office'i değildir. Universal link / app link MVP'de yok.                                                                           | E1 ve E3 aynı public web yüzeyini kullanır; backend (NestJS) sunucu tarafında render eder, ayrı web repo yok. Dev build şartı ortadan kalkar.                              |
| K15 | Onboarding ilerlemesi adım numarası olarak saklanmaz. Sunucuda yalnızca `onboardingCompletedAt` tutulur; eksik adımlar veriden türetilir.                                                                                                                                                                                                     | "Kurulumu tamamla" kartı her cihazda aynı sonucu verir (KUR-04).                                                                                                           |
| K16 | Müşteri başına **tek telefon** vardır ve işletme içinde benzersizdir. İkinci telefon alanı yoktur.                                                                                                                                                                                                                                            | Telefon müşteri arama anahtarıdır; mükerrer kayıt birleştirme gerekmez.                                                                                                    |
| K17 | Hayvan uyarı etiketleri **sabit bir listeden** seçilir. Müşteri etiketleri serbest metindir.                                                                                                                                                                                                                                                  | Uyarı etiketleri API'de enum; takvimde ikon olarak gösterilebilir (HAY-02).                                                                                                |
| K18 | Aşı için son yapılma tarihi girilir. Geçerlilik bitişi otomatik **son tarih + 12 ay** hesaplanır ve elle düzeltilebilir.                                                                                                                                                                                                                      | Durum saklanmaz, `validUntil` üzerinden hesaplanır (HAY-03).                                                                                                               |
| K19 | Hayvan türleri yalnızca **Köpek** ve **Kedi**.                                                                                                                                                                                                                                                                                                | Tür enum; ırk önerileri türe göre (HAY-01).                                                                                                                                |
| K20 | Hayvan silinmez, **arşivlenir**. Kalıcı silme yalnızca müşteri silme ile olur (E8).                                                                                                                                                                                                                                                           | Arşivli hayvana randevu ve hatırlatma yok (HAY-05).                                                                                                                        |
| K21 | Intake Form Petzibu'nun kendi formudur. Google Forms ürünün parçası değildir; yalnızca ürün öncesi araştırma aracı olarak kullanılabilir.                                                                                                                                                                                                     | Form public web yüzeyinde (K14) yaşar.                                                                                                                                     |
| K22 | İki link türü vardır: işletme başına **genel link** ve müşteriye bağlı **kişiye özel link**. İkisi aynı mekanizmayla çalışır.                                                                                                                                                                                                                 | Tek `intakeLink` modeli; `customerId` boşsa genel (INT-01, INT-02).                                                                                                        |
| K23 | Her form, kaynağı ne olursa olsun, owner **onayından geçmeden** kayıt oluşturmaz veya güncellemez.                                                                                                                                                                                                                                            | Başvuru kutusu ve onay akışı (INT-04, INT-05).                                                                                                                             |
| K24 | Intake Form **sabit şablondur**. Alan açma/kapama ayarı yoktur (K6'nın yerine geçer). Zorunlu alanlar yalnızca: sahip ad soyad, telefon, hayvan adı, hayvan türü.                                                                                                                                                                             | Ayar ekranı yok; form tek sürüm (INT-03).                                                                                                                                  |
| K25 | Aşı karnesi fotoğrafı istenmez; yalnızca aşı tarihleri sorulur.                                                                                                                                                                                                                                                                               | Form fotoğrafları yalnızca hayvan fotoğrafıdır (INT-03).                                                                                                                   |
| K26 | **Geçmiş tarihe** randevu girilebilir. Geçmiş tarihli randevu da "Bekliyor" durumunda oluşur.                                                                                                                                                                                                                                                 | Geçmiş randevular için çakışma ve saat uyarısı verilmez (RAN-04). Kayıt sonrası "Bu randevu ne oldu?" sorulur (E5).                                                        |
| K27 | İptal edilen randevular takvimde **gizlenir**. "Gelmedi" olarak işaretlenen randevular görünür kalır.                                                                                                                                                                                                                                         | İptaller yalnızca müşteri geçmişinde görünür (RAN-06).                                                                                                                     |
| K28 | Başlangıç saati **15 dakikalık** adımlarla seçilir. Bitiş saati hesaplandığı için 5 dakikalık değerlere düşebilir.                                                                                                                                                                                                                            | Hizmet süreleri 5 dk adımlı kalır (KUR-07).                                                                                                                                |
| K29 | Mobilde hafta görünümü **gün satırlarından oluşan kompakt bir liste**dir. Zaman çizelgesi biçimindeki hafta görünümü yalnızca web'dedir.                                                                                                                                                                                                      | Mobil takvim: gün çizelgesi + hafta listesi (RAN-01).                                                                                                                      |
| K30 | Hayvanlar sırayla yapılır. Randevunun süresi, bütün hizmet satırlarının sürelerinin toplamıdır.                                                                                                                                                                                                                                               | Bitiş saati hesaplanır, elle düzeltilebilir (RAN-02).                                                                                                                      |
| K31 | Uyarılar (çakışma, çalışma saati dışı, kapalı gün) hiçbir zaman kaydı engellemez.                                                                                                                                                                                                                                                             | Sunucu `422 APPOINTMENT_WARNINGS` döner, istemci onayla tekrar gönderir (RAN-04).                                                                                          |
| K32 | Randevu satırı fiyatı **tek kuralla** gelir: hayvan + hizmet için owner'ın bilerek sabitlediği **özel fiyat** varsa o, yoksa kademe fiyatı. Son ödenen fiyat otomatik uygulanmaz, yalnızca bilgi olarak gösterilir.                                                                                                                           | Özel fiyat hayvan profilinde listelenir ve kaldırılabilir (HAY-01, RAN-02). Zam ve kademe değişikliği doğru yansır.                                                        |
| K33 | Önce/sonra **kolajı telefonda, paylaşım anında** üretilir; saklanmaz. Tek şablon: 4:5 dikey, üstte Önce, altta Sonra, köşede salon adı.                                                                                                                                                                                                       | Backend yalnızca orijinal fotoğrafları tutar. Hayvan başına bir kolaj, hepsi tek paylaşımda (E5).                                                                          |
| K34 | Ödeme gecikince 7 gün **Ödeme bekliyor**, sonra **Askıda**. Askıda veri salt okunur kalır, dışa aktarma ve KVKK işlemleri çalışır, yazma istekleri `403 TENANT_SUSPENDED` ile reddedilir. Askı en fazla 12 ay; silmeden 30 gün önce haber verilir. Ödeme MVP'de **manuel** (havale veya link); hesabı Petzibu ekibi ADM-02'den aktifleştirir. | "Verilerin güvende" vaadi (ADM-02, E8). Hatırlatmalar ve intake linki askıda durur (E3, E6). Ödeme entegrasyonu yok.                                                       |
| K35 | KVKK rolleri: owner verisi için veri sorumlusu **Petzibu**; müşteri ve hayvan verisi için veri sorumlusu **salon**, Petzibu **veri işleyen**.                                                                                                                                                                                                 | Intake aydınlatma metni işletme bazlı, salon adı ve adresi otomatik dolar (INT-03). Owner davette veri işleme sözleşmesini de onaylar (KUR-01). Metinler E8'de tanımlanır. |
| K36 | Hizmetin bir **türü** vardır: Köpek / Kedi / İkisi. Randevu formundaki hizmet chip'leri hayvanın türüne göre süzülür.                                                                                                                                                                                                                         | KUR-07'ye tek alan; kedi hizmetlerinde "tüm boyutlar için aynı" varsayılan açık (RAN-02).                                                                                  |
| K37 | Tamamlanan randevu **geri alınamaz**. Tutar ve kalem düzeltmeleri E7'deki döküm üzerinden yapılır. | Durum geçiş tablosunda Tamamlandı'dan çıkış yok (OPR-01). |
| K38 | "Geldi" adımı **atlanabilir**: Bekliyor veya Onaylandı randevu doğrudan tamamlanabilir, geçmiş tarihli randevular dahil. | "Geldi mi?" sorusu Tamamlandı / Gelmedi seçenekleriyle (OPR-01, RAN-02). |
| K39 | Rebook hatırlatması **müşteri bazındadır**, hayvan bazında değil. | Müşteride `rebookReminderAt` (OPR-06, E6). |
| K40 | Bakım raporu tamamlama akışından **bağımsızdır**; zorunlu adım değildir. | Rapor başlatılınca Bekliyor/Onaylandı randevu otomatik Geldi olur; paylaşım hayvan başına tek görsel + metin (OPR-04). |
| K41 | Borç tahsilatı **en eski dökümden başlayarak** dökümlere otomatik dağıtılır. | Toplu tahsilat döküm başına ayrı ödeme kaydı üretir (KAS-04). |
| K42 | Döküm **PDF olarak backend'de** üretilir; mobilde paylaşım menüsü, web'de indirme. Metin versiyonu yok. | `GET /statements/:id/pdf`, `lib/api.ts` içine `downloadFile()` (KAS-06). |
| K43 | İndirim yalnızca **toplam tutara**; tutar veya yüzde. | Satır bazında indirim yok (KAS-02). |
| K44 | Gider kategorileri **sabit liste**: Malzeme, Kira, Faturalar, Ekipman, Diğer. | Kategori ayarı yok (KAS-07). |
| K45 | Fiyatlar KDV dahil; vergi hesaplanmaz. Döküm mali belge değildir ve bunu üzerinde belirtir. | PDF alt bilgisi (KAS-06). |

---

## Sonraki fazlar

### Faz 2

- **Rakip analizi v2 sonrası öneriler (karar bekliyor):** online randevu "talep gönder, salon onaylasın" modeliyle Faz 3'ten Faz 2'ye; paket/seans kartı işaretli bakiye üzerine Faz 2'ye; otomatik WhatsApp maliyeti fiyata dahil. Bkz. [docs/resources/2026-09-23-pet-kuafor-rakip-analizi/petzibu-rakip-analizi-v2.md](../resources/2026-09-23-pet-kuafor-rakip-analizi/petzibu-rakip-analizi-v2.md) §7.
- **Personel modülü:** personel takvimi, rol ve yetki, vardiya yönetimi, mesai takibi, prim/komisyon/bahşiş ayarları, bordro, personel değerlendirme ve sıralama
- **Otomatik WhatsApp:** WhatsApp Business API, Meta şablon onayı
- **Web back office:** Excel'den import, detaylı raporlar
- **Personel ataması:** randevu satırına personel atama

### Faz 3

- Online rezervasyon ve slot bazlı müsaitlik
- Ön ödeme, kart saklama, provizyon
- Bekleme listesi (waitlist)
- Yarım kalan rezervasyon takibi (leads/abandon)
- Pazarlama, kampanya ve indirimler (İYS kaydı gerekiyor)
- Pet sahibi uygulaması ve pazaryeri

### Kapsam dışı

- Pet oteli (lodging) ve kreş (playgroup)

---

## Story formatı

```
**ID · Başlık** · Platform
*Salon sahibi olarak …, böylece ….*
- Kabul kriteri
- Kabul kriteri
```
