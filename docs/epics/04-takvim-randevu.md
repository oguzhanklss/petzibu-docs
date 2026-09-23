# E4 · Takvim & Randevu

| Durum     | Faz | Platform                                       | Bağımlılık                                              |
| --------- | --- | ---------------------------------------------- | ------------------------------------------------------- |
| Yapılacak | MVP | Mobil + Web (web'de hafta görünümü varsayılan) | [E1](01-kurulum-isletme.md), [E2](02-musteri-hayvan.md) |

> Story'ler: Kesinleşti v1.4 · 23 Eylül 2026

## Amaç

Randevu telefonda, müşteriyi bekletmeden oluşturulabilsin.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [README](index.md#alınan-kararlar).

| #        | Karar                                                                                                                                                 |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| K1       | Bir randevu bir müşteriye aittir ve içinde **bir veya birden fazla hayvan** bulunur. Her hayvanın altında bir veya birden fazla hizmet satırı vardır. |
| K4 / K12 | Hizmet fiyatı ve süresi hayvanın boyut kademesinden gelir.                                                                                            |
| K7       | Tekrarlayan randevu MVP'de yok.                                                                                                                       |
| K20      | Arşivlenmiş hayvan randevuya eklenemez.                                                                                                               |
| K26      | **Geçmiş tarihe** randevu girilebilir. Geçmiş tarihli randevu da "Bekliyor" durumunda oluşur.                                                         |
| K27      | İptal edilen randevular takvimde **gizlenir**. "Gelmedi" olarak işaretlenen randevular görünür kalır.                                                 |
| K28      | Başlangıç saati **15 dakikalık** adımlarla seçilir. Bitiş saati hesaplandığı için 5 dakikalık değerlere düşebilir.                                    |
| K29      | Mobilde hafta görünümü **gün satırlarından oluşan kompakt bir liste**dir. Zaman çizelgesi biçimindeki hafta görünümü yalnızca web'dedir.              |
| K30      | Hayvanlar sırayla yapılır. Randevunun süresi, bütün hizmet satırlarının sürelerinin toplamıdır.                                                       |
| K31      | Uyarılar (çakışma, çalışma saati dışı, kapalı gün) hiçbir zaman kaydı engellemez.                                                                     |
| K32      | Satır fiyatı: hayvan + hizmet özel fiyatı varsa o, yoksa kademe fiyatı. Son fiyat yalnızca bilgi.                                                     |
| K36      | Hizmet chip'leri hayvanın türüne göre süzülür (Köpek / Kedi / İkisi).                                                                                 |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

| ID     | Başlık                        | Platform    | Durum     |
| ------ | ----------------------------- | ----------- | --------- |
| RAN-01 | Takvim görünümü               | Mobil + Web | Yapılacak |
| RAN-02 | Randevu oluşturma             | Mobil + Web | Yapılacak |
| RAN-03 | Boş saate dokunarak randevu   | Mobil + Web | Yapılacak |
| RAN-04 | Uyarılar                      | Mobil + Web | Yapılacak |
| RAN-05 | Randevu düzenleme ve erteleme | Mobil + Web | Yapılacak |
| RAN-06 | İptal                         | Mobil + Web | Yapılacak |
| RAN-07 | Ziyarete özel not             | Mobil + Web | Yapılacak |

---

## Takvim

**RAN-01 · Takvim görünümü** · Mobil + Web
_Salon sahibi olarak günümü ve haftamı tek bakışta görmek istiyorum, böylece sıradaki işi ve boş saatleri bilirim._

- **Gün görünümü** (mobilde varsayılan):
  - Zaman çizelgesi biçimindedir; randevu bloğunun yüksekliği süresiyle orantılıdır.
  - Bugün gösteriliyorsa şu anki saati gösteren bir çizgi vardır.
  - Aynı saate denk gelen randevular yan yana, daraltılmış sütunlar olarak gösterilir.
  - Günler ok tuşları veya üstteki gün şeridiyle değiştirilir. "Bugün" butonu tek dokunuşla bugüne döner.
- **Hafta görünümü:**
  - Mobilde her gün bir satırdır. Satırda günün adı, randevu sayısı ve randevuların saat ile hayvan adı listelenir. Satıra dokununca o günün gün görünümü açılır (K29).
  - Web'de varsayılan görünümdür ve zaman çizelgesi biçimindedir (7 sütun).
- **Randevu bloğu** şunları gösterir:
  - Hayvan adları ("Paşa, Minnoş")
  - Müşteri adı ve hizmetler
  - Durum chip'i
  - Uyarı etiketi ikonu (HAY-02), aşı sorunu ikonu (HAY-03), çakışma ikonu (RAN-04), not ikonu (RAN-07)
- Bloğa dokununca randevu detayı açılır (OPR-02).
- Bitiş saati geçmiş ve durumu Bekliyor/Onaylandı olan blokta "Geldi mi?" işareti görünür (OPR-01).
- İptal edilen randevular gösterilmez; "Gelmedi" olanlar gösterilir (K27).
- Çalışma saatleri dışı gri görünür (KUR-06). Özel kapalı gün gri ve etiketli görünür (KUR-09).
- Kurulum eksikse takvimin üstünde "Kurulumu tamamla" kartı görünür (KUR-04).
- Sağ altta yeni randevu butonu (FAB) bulunur.

---

## Randevu oluşturma

**RAN-02 · Randevu oluşturma** · Mobil + Web
_Salon sahibi olarak telefondaki müşteriyi bekletmeden randevu oluşturmak istiyorum._

- **Müşteri**
  - Telefon veya adla aranır (MUS-01 arama kuralları).
  - Bulunamazsa aynı ekrandan ad ve telefonla yeni müşteri eklenir (MUS-02). Arama kutusuna yazılmış numara bu forma dolu gelir.
  - Müşteri seçilince gelmedi sayısı ve borcu sıfırdan büyükse formda bilgi şeridi görünür ("2 kez gelmedi · 450 ₺ borç"). Kayıt engellenmez.
- **Hayvanlar**
  - Müşterinin arşivlenmemiş hayvanları chip olarak gösterilir; birden fazlası seçilebilir.
  - "+ Hayvan" ile yeni hayvan **yalnızca ad ve türle** eklenebilir (HAY-01). Diğer bilgiler sonradan veya intake formundan (INT-02) tamamlanır.
  - Seçilen hayvanların sırası, bakım sırasıdır.
- **Hizmetler**
  - Seçilen her hayvana bir veya birden fazla hizmet eklenir. Hizmet chip'leri hayvanın türüne göre süzülür: kedi için yalnızca "Kedi" ve "İkisi" türündeki hizmetler görünür (K36).
  - Hizmetin süresi ve fiyatı hayvanın boyut kademesinden otomatik gelir (KUR-07).
  - Hayvanın kademesi boşsa form içinde "Boyutu?" diye sorulur (Küçük / Orta / Büyük). Seçilen kademe hayvanın kaydına da yazılır.
- **Tarih ve saat**
  - Varsayılan tarih bugündür. Başlangıç saati 15 dakikalık adımlarla seçilir (K28).
  - Geçmiş tarih seçilebilir (K26). Geçmiş tarihli randevu Bekliyor olarak kaydedilir; bitiş saati geçmiş olduğu için detayda ve takvim bloğunda hemen "Geldi mi?" sorusu görünür: Tamamlandı / Gelmedi (OPR-01, K38). "Tamamlandı" tahsilat adımına götürür (E7).
- **Süre ve bitiş**
  - Toplam süre bütün satırların sürelerinin toplamıdır (K30). Bitiş saati otomatik hesaplanır ve formda gösterilir ("Bitiş: 15:15").
  - Bitiş saati elle uzatılıp kısaltılabilir. Hizmet veya hayvan sonradan değişirse bitiş saati yeniden hesaplanır ve değişiklik kullanıcıya gösterilir.
- **Fiyat**
  - Satır fiyatı tek kuralla gelir (K32): hayvan + hizmet için özel fiyat varsa o, yoksa hayvanın kademe fiyatı. Son ödenen fiyat otomatik uygulanmaz.
  - Her satırın fiyatı ayrı ayrı düzenlenebilir. Toplam tutar ekranın altında görünür.
  - Owner satır fiyatını değiştirince "Paşa için bu fiyatı sabitle" seçeneği çıkar. İşaretlenirse hayvan + hizmet özel fiyatı olarak kaydedilir (HAY-01); işaretlenmezse değişiklik yalnızca bu randevu içindir.
  - Bu hayvan + hizmet için son ödenen fiyat, gelen fiyattan farklıysa satırda yalnızca bilgi olarak görünür: "Geçen sefer 380 ₺".
  - Satırın hizmet adı, süresi ve fiyatı randevuya o anki değerleriyle kopyalanır. Hizmet sonradan değişse de bu randevu etkilenmez.
- **Uyarılar**
  - Seçilen hayvanda aşı sorunu veya uyarı etiketi varsa formda gösterilir; kayıt engellenmez.
  - Kaydetmeden önce RAN-04'teki kontroller yapılır.
- Yeni randevunun durumu "Bekliyor"dur. Durum akışı E5'tedir.
- Kayıttan sonra, müşteri bu formda yeni eklendiyse "Formu gönder" önerisi gösterilir (INT-02).

**RAN-03 · Boş saate dokunarak randevu** · Mobil + Web
_Salon sahibi olarak takvimde boş bir saate dokunup randevu formunu o tarih ve saat dolu olarak açmak istiyorum._

- Dokunulan saat, en yakın önceki 15 dakikalık dilime yuvarlanır.
- **Walk-in:** yeni randevu butonunda "Şimdi" seçeneği vardır. Form, tarih bugün ve saat şu an (önceki 15 dakikalık dilime yuvarlanmış) olarak açılır; randevusuz gelen müşteri için saat seçme adımı atlanır.
- Kapalı bir güne veya çalışma saati dışına dokunulursa form yine açılır; uyarı kaydetme sırasında verilir (RAN-04).

**RAN-04 · Uyarılar** · Mobil + Web
_Salon sahibi olarak sorunlu bir randevu girersem uyarılmak istiyorum, ama yine de kaydedebilmeliyim._

- Kontrol edilen durumlar:
  - Başka bir sonuçlanmamış randevuyla zaman çakışması
  - Çalışma saatleri dışı (KUR-06)
  - Haftalık kapalı gün veya özel kapalı gün (KUR-09)
- Uyarılar kaydetmeden önce tek bir ekranda listelenir. Owner "Yine de kaydet" ile devam edebilir veya geri dönüp düzeltebilir (K31).
- Çakışmalı kaydedilen randevunun detayında uyarı şeridi kalır (OPR-02); takvim bloğunda çakışma ikonu görünür.
- Çakışma, diğer randevu iptal edilir veya taşınırsa kendiliğinden kalkar.
- Geçmiş tarihli randevular için çakışma ve çalışma saati uyarısı verilmez.

---

## Randevu değişiklikleri

**RAN-05 · Randevu düzenleme ve erteleme** · Mobil + Web
_Salon sahibi olarak randevunun tarihini, saatini, hayvanlarını ve hizmetlerini değiştirebilmek istiyorum._

- Yalnızca sonuçlanmamış randevular (Bekliyor, Onaylandı, Geldi) düzenlenebilir. Tamamlanmış randevuya hizmet veya ek ücret eklemek E5 ve E7'nin konusudur.
- Tarih ve saat değişince RAN-04'teki kontroller yeniden yapılır.
- Hayvan eklenebilir; hizmet eklenip çıkarılabilir; satır fiyatı değiştirilebilir.
- **Hayvan çıkarma:**
  - Bir hayvan, bütün hizmet satırlarıyla birlikte randevudan çıkarılabilir.
  - Süre, bitiş saati ve toplam tutar yeniden hesaplanır; randevunun başlangıç saati değişmez.
  - Randevuda tek hayvan kalmışsa o hayvan çıkarılamaz; bunun yerine randevuyu iptal etme seçeneği sunulur (RAN-06).
- Erteleme ayrı bir durum değildir; yalnızca tarih ve saat değişir. Hatırlatmalar (E6) yeni saate göre çalışır.
- Müşteri değiştirilemez. Yanlış müşteriye açılmış randevu iptal edilip yeniden oluşturulur.

**RAN-06 · İptal** · Mobil + Web
_Salon sahibi olarak randevuyu iptal edebilmek ve iptali kimin yaptığını kaydetmek istiyorum._

- Yalnızca sonuçlanmamış randevular iptal edilebilir.
- İptal ederken "Müşteri iptal etti" veya "Salon iptal etti" seçimi zorunludur; isteğe bağlı bir sebep notu eklenebilir.
- Yalnızca "Müşteri iptal etti" seçilen iptaller müşterinin iptal sayısına (MUS-04) yansır.
- İptal edilen randevu takvimde gizlenir (K27); müşteri detayındaki randevu geçmişinde "İptal" etiketiyle ve kimin iptal ettiği bilgisiyle görünür.
- İptal geri alınamaz; gerekirse yeni randevu oluşturulur.
- İptal edilen randevu için hatırlatma gönderilmez (E6).

**RAN-07 · Ziyarete özel not** · Mobil + Web
_Salon sahibi olarak "bu sefer kısa kesilecek" gibi yalnızca bu randevuya ait bir not düşebilmek istiyorum._

- Not, randevu oluştururken veya sonradan eklenebilir.
- Randevu detayında görünür; takvim bloğunda not ikonu çıkar.
- Hayvanın kalıcı notlarından (huy, alerji, tıraş tercihi) ayrı tutulur.

---

## Kapsam dışı

- Tekrarlayan randevu (K7)
- Sürükle-bırak ile randevu taşıma (hafta görünümü dahil)
- Ay görünümü
- Personel takvimi ve personel ataması (Faz 2)
- Online rezervasyon ve "ilk boş saati bul" önerisi (Faz 3)
- İptali geri alma
- Randevunun müşterisini değiştirme
- Hayvanların paralel yapılması (iki hayvana aynı anda bakım)

## Teknik notlar

| Konu            | Karar                                                                                                                                                                                                                                                                                                                                       | Story          |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| Veri modeli     | `appointment` (`customerId`, `startAt`, `endAt`, `status`, `note`, `cancelledBy`, `cancelReason`) → `appointmentPet` (`petId`, `position`) → `appointmentLine` (`type: service \| extra`, `serviceId` veya `extraChargeId`, `tier`, `name`, `durationMin`, `price`). Ad, süre ve fiyat satıra kopyalanır (snapshot). Ek ücret satırında `durationMin` 0 (OPR-03). `price` kuruş cinsinden tam sayıdır.                           | RAN-02         |
| Fiyat kaynağı   | Sunucu satır fiyatını `petServicePrice` → kademe sırasıyla hesaplar ve yanıtta `priceSource: custom \| tier` ile `lastPaidPrice` (nullable) döner. İstemci hesaplamaz, yalnızca gösterir.                                                                                                                                                   | RAN-02         |
| Çevrimdışı      | MVP'de yok ([RULES.md](../rules.md)). Faz 2'de bugünün takvimi ve randevu detayları salt okunur çevrimdışı açılabilsin diye TanStack Query önbelleğinin kalıcı yapılmasına kapı açık tutulur; token asla önbelleğe girmez.                                                                                                               | RAN-01         |
| Doğrulama       | En az bir hayvan ve her hayvanda en az bir hizmet satırı zorunludur (K1). Aynı hayvan bir randevuda bir kez bulunur. API de aynı kuralı uygular.                                                                                                                                                                                            | RAN-02, RAN-05 |
| Bitiş saati     | `endAt` saklanır. Satır değişince `startAt + toplam süre` ile yeniden hesaplanır; elle yapılan değişiklik yalnızca bir sonraki satır değişikliğine kadar geçerlidir.                                                                                                                                                                        | RAN-02, RAN-05 |
| Uyarılar        | Uyarıları sunucu hesaplar. Uyarı varsa ve istekte `confirmWarnings: true` yoksa API `422 APPOINTMENT_WARNINGS` döner; uyarılar hata zarfındaki `errors[]` içinde, her biri kendi koduyla (`OVERLAP`, `OUTSIDE_HOURS`, `CLOSED_DAY`) ve mesajıyla gelir. İstemci listeyi gösterir ve onayla tekrar gönderir. Tek doğruluk kaynağı sunucudur. | RAN-04         |
| Çakışma         | Yalnızca sonuçlanmamış randevular (Bekliyor, Onaylandı, Geldi) çakışma hesabına girer. Çakışma `endAt` üzerinden hesaplanır; elle uzatılmış bitiş de sayılır.                                                                                                                                                                               | RAN-04         |
| Takvim sorgusu  | Gün ve hafta görünümü `dateFrom` + `dateTo` ile sayfasız liste çeker (`envelope(z.array(appointment))`, bkz. [RULES.md](../rules.md)). İptal edilenler sunucuda elenir.                                                                                                                                                                  | RAN-01         |
| Zaman           | Zamanlar UTC saklanır; işletmenin saat dilimi Europe/Istanbul olarak gösterilir. 15 dakikalık yuvarlama yerel saate göre yapılır.                                                                                                                                                                                                           | Tümü           |
| Durum enum'u    | `pending \| confirmed \| arrived \| completed \| no_show \| cancelled`. "Onaylandı" = `confirmed`. [RULES.md](../rules.md) bu enum'a göre güncellendi.                                                                                                                                                                                   | RAN-04, RAN-05 |
| Durum kısıtları | Düzenleme ve iptal isteği sonuçlanmış randevu için API'de de reddedilir (`409 APPOINTMENT_FINALIZED`).                                                                                                                                                                                                                                      | RAN-05, RAN-06 |
| Arşiv           | Arşivlenmiş hayvan (`archivedAt` dolu) randevuya eklenmek istenirse API reddeder.                                                                                                                                                                                                                                                           | RAN-02, RAN-05 |
| E2 alanları     | E4 ve E5 ile `lastVisitAt` (son tamamlanan randevu) ve `isNew` (tamamlanmış randevu yok) dolmaya başlar (MUS-01).                                                                                                                                                                                                                           | —              |
| Kod yeri        | `features/appointments/` (api, queries, schema, components). Takvim ekranı `app/(tabs)/index.tsx` yalnızca kompozisyon.                                                                                                                                                                                                                     | Tümü           |

## Diğer epic'lere bağlantılar

| Buradan        | Oraya                  | Konu                                                                |
| -------------- | ---------------------- | ------------------------------------------------------------------- |
| RAN-01         | KUR-04, KUR-06, KUR-09 | Kurulum kartı, çalışma saatleri, kapalı günler                      |
| RAN-01         | HAY-02, HAY-03         | Takvim bloğunda uyarı ve aşı ikonları                               |
| RAN-02         | MUS-01, MUS-02         | Müşteri arama ve hızlı ekleme                                       |
| RAN-02         | HAY-01, KUR-07         | Hızlı hayvan ekleme, boyut kademesi ve fiyat                        |
| RAN-02         | INT-02                 | Yeni müşteriye "Formu gönder" önerisi                               |
| RAN-05         | HAY-05                 | Arşivlemeden önce hayvanı randevudan çıkarma                        |
| RAN-06         | MUS-04                 | Müşteri iptal sayısı                                                |
| RAN-01, RAN-05 | E5                     | Durum akışı, randevu detayı, tamamlanmış randevu                    |
| RAN-01, RAN-02 | OPR-01 | "Geldi mi?" sorusu: takvim bloğunda ve geçmiş tarihli yeni randevuda |
| RAN-05, RAN-06 | E6                     | Hatırlatmaların yeni saate göre çalışması ve iptalde durması        |

## Açık sorular

- Yok.

## Tasarımla uyuşmazlıklar ("Yeni Randevu" ekranı)

Story'ler kesin, tasarım güncellenecek:

- **"Görüşme aktif · Müşteri hattı 01:42"** tasarımdan çıkar. iOS üçüncü parti uygulamaya aktif aramayı göstermez; Android'de özel izin ve Play onayı gerekir. Story'lerde de yok.
- **"Masa 2 · Uygun"** çıkar. Masa veya kapasite modeli yok; RAN-04 çakışmayı yalnızca uyarı olarak ele alır.
- **Tek "Ücret" alanı** yerine hayvan ve hizmet bazında satır fiyatları, toplam altta (RAN-02).
