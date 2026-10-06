# E4 · Takvim & Randevu

| Durum     | Faz | Platform                                       | Bağımlılık                                              |
| --------- | --- | ---------------------------------------------- | ------------------------------------------------------- |
| Yapılacak | MVP | Mobil + Web (web'de hafta görünümü varsayılan) | [E1](01-kurulum-isletme.md), [E2](02-musteri-hayvan.md) |

> Story'ler: Kesinleşti

## Amaç

Randevu telefonda, müşteriyi bekletmeden oluşturulabilsin.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [Epic Haritası](index.md#alınan-kararlar).

| #                               | Başlık                                                                                   |
| ------------------------------- | ---------------------------------------------------------------------------------------- |
| [K1](index.md#alınan-kararlar)  | Bir randevu içinde birden fazla hayvan olabilir.                                         |
| [K4](index.md#alınan-kararlar)  | Hizmetlerde boyut kademesi var: küçük / orta / büyük için ayrı süre ve fiyat.            |
| [K7](index.md#alınan-kararlar)  | Tekrarlayan randevu MVP'de yok.                                                          |
| [K12](index.md#alınan-kararlar) | Boyut kademesi kilo eşikleri sabit: küçük < 10 kg, orta 10–25 kg, büyük > 25 kg.         |
| [K20](index.md#alınan-kararlar) | Hayvan silinmez, arşivlenir.                                                             |
| [K26](index.md#alınan-kararlar) | Geçmiş tarihe randevu girilebilir.                                                       |
| [K27](index.md#alınan-kararlar) | İptal edilen randevular takvimde gizlenir.                                               |
| [K28](index.md#alınan-kararlar) | Başlangıç saati 15 dakikalık adımlarla seçilir.                                          |
| [K29](index.md#alınan-kararlar) | Mobilde hafta görünümü gün satırlarından oluşan kompakt bir listedir.                    |
| [K30](index.md#alınan-kararlar) | Hayvanlar sırayla yapılır; randevunun süresi hizmet satırlarının sürelerinin toplamıdır. |
| [K31](index.md#alınan-kararlar) | Uyarılar (çakışma, çalışma saati dışı, kapalı gün) hiçbir zaman kaydı engellemez.        |
| [K32](index.md#alınan-kararlar) | Randevu satırı fiyatı tek kuralla gelir: özel fiyat varsa o, yoksa kademe fiyatı.        |
| [K36](index.md#alınan-kararlar) | Hizmetin bir türü vardır: Köpek / Kedi / İkisi.                                          |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

Backend tarafı BS3'te kapandı (takvim, fiyat önizlemesi, uyarı motoru, oluşturma, okuma, düzenleme, iptal); story'leri `Tamamlandı`ya mobil sprint (MS3) taşır. RAN-03'ün backend işi yok. Aynı sprintte E1 ve E2'nin randevu bekleyen alanları da doldu (BS3-08).

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
- Geçmiş tarihli randevular için hiçbir uyarı verilmez: ne çakışma, ne çalışma saati, ne kapalı gün (K26).

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

| Konu                 | Karar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Story          |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| Veri modeli          | `appointment` (`customerId`, `startAt`, `endAt`, `status`, `note`, `cancelledBy`, `cancelReason`) → `appointmentPet` (`petId`, `position`) → `appointmentLine` (`type: service \| extra`, `serviceId` veya `extraChargeId`, `tier`, `name`, `durationMin`, `price`). Ad, süre ve fiyat satıra kopyalanır (snapshot). Ek ücret satırında `durationMin` 0 (OPR-03). `price` kuruş cinsinden tam sayıdır.                                                                                                         | RAN-02         |
| Fiyat kaynağı        | Sunucu satır fiyatını `petServicePrice` → kademe sırasıyla hesaplar ve yanıtta `priceSource: custom \| tier` ile `lastPaidPrice` (nullable) döner. İstemci hesaplamaz, yalnızca gösterir. `priceSource` **kuralın kaynağını** söyler, owner'ın bu randevuya özel elle düzeltmesini değil: satır fiyatı elle değiştirildiğinde `priceSource` değişmez — o düzeltmeyi özel fiyata çevirmek `pinPrice`'ın işidir. Böylece alan satırda saklanmaz, kaydedilmiş satır okunurken de aynı şekilde yeniden hesaplanır. | RAN-02         |
| Çevrimdışı           | MVP'de yok ([mobil RULES.md](../rules-mobile.md)). Faz 2'de bugünün takvimi ve randevu detayları salt okunur çevrimdışı açılabilsin diye TanStack Query önbelleğinin kalıcı yapılmasına kapı açık tutulur; token asla önbelleğe girmez.                                                                                                                                                                                                                                                                        | RAN-01         |
| Doğrulama            | En az bir hayvan ve her hayvanda en az bir hizmet satırı zorunludur (K1). Aynı hayvan bir randevuda bir kez bulunur. API de aynı kuralı uygular.                                                                                                                                                                                                                                                                                                                                                               | RAN-02, RAN-05 |
| Bitiş saati          | `endAt` saklanır. Satır değişince `startAt + toplam süre` ile yeniden hesaplanır; elle yapılan değişiklik yalnızca bir sonraki satır değişikliğine kadar geçerlidir.                                                                                                                                                                                                                                                                                                                                           | RAN-02, RAN-05 |
| Uyarılar             | Uyarıları sunucu hesaplar. Uyarı varsa ve istekte `confirmWarnings: true` yoksa API `422 APPOINTMENT_WARNINGS` döner; uyarılar hata zarfındaki `errors[]` içinde, her biri kendi koduyla (`OVERLAP`, `OUTSIDE_HOURS`, `CLOSED_DAY`) ve mesajıyla gelir. İstemci listeyi gösterir ve onayla tekrar gönderir. Tek doğruluk kaynağı sunucudur. `startAt < şimdi` ise sunucu uyarı hesaplamaz (K26).                                                                                                               | RAN-04         |
| Çakışma              | Yalnızca sonuçlanmamış randevular (Bekliyor, Onaylandı, Geldi) çakışma hesabına girer. Çakışma `endAt` üzerinden hesaplanır; elle uzatılmış bitiş de sayılır. Aralık `[startAt, endAt)`: bitişi bir sonrakinin başlangıcına değen randevu çakışmaz. Kaydetmeden önce sorulan çakışmanın penceresi **adayın kendi aralığıdır** ve takvim sorgusundan bağımsızdır; takvim ikonu ise dönen aralığın içinde hesaplanır, çünkü orada ekranda görünenle tutarlı olmalıdır.                                                                                                                                                                                                                                                                                                                                                  | RAN-04         |
| Takvim sorgusu       | Gün ve hafta görünümü `dateFrom` + `dateTo` ile sayfasız liste çeker (`envelope(z.array(appointment))`, bkz. [mobil RULES.md](../rules-mobile.md)). İptal edilenler sunucuda elenir.                                                                                                                                                                                                                                                                                                                           | RAN-01         |
| Zaman                | Zamanlar UTC saklanır; işletmenin saat dilimi Europe/Istanbul olarak gösterilir. 15 dakikalık yuvarlama yerel saate göre yapılır.                                                                                                                                                                                                                                                                                                                                                                              | Tümü           |
| Durum enum'u         | `pending \| confirmed \| arrived \| completed \| no_show \| cancelled`. "Onaylandı" = `confirmed`. [mobil RULES.md](../rules-mobile.md) bu enum'a göre güncellendi.                                                                                                                                                                                                                                                                                                                                            | RAN-04, RAN-05 |
| Sonuçlanmış randevu  | **Sonuçlanmamış** = `pending`, `confirmed`, `arrived`. **Sonuçlanmış** = `completed`, `no_show`, `cancelled`. E4 ve E5'teki bütün "sonuçlanmış / sonuçlanmamış" ifadeleri bu tanımı kullanır. `no_show` → `pending` geri alması bir durum geçişidir (OPR-01), düzenleme değildir.                                                                                                                                                                                                                              | Tümü           |
| Durum kısıtları      | Düzenleme ve iptal isteği sonuçlanmış randevu için API'de de reddedilir (`409 APPOINTMENT_FINALIZED`).                                                                                                                                                                                                                                                                                                                                                                                                         | RAN-05, RAN-06 |
| Arşiv                | Arşivlenmiş hayvan (`archivedAt` dolu) randevuya eklenmek istenirse API reddeder.                                                                                                                                                                                                                                                                                                                                                                                                                              | RAN-02, RAN-05 |
| E2 alanları          | E4 ve E5 ile `lastVisitAt` (son tamamlanan randevu) ve `isNew` (tamamlanmış randevu yok) dolmaya başlar (MUS-01).                                                                                                                                                                                                                                                                                                                                                                                              | —              |
| Fiyat önizleme       | Fiyat ve süre çözümü tek bir servis metodunda yaşar; `POST /appointments/quote` onu hiçbir şey yazmadan çağırır ve satır satır `price`, `priceSource`, `durationMinutes`, `lastPaidPrice` ile toplam süre ve `endAt` döner. Form fiyatı kaydetmeden önce buradan okur; istemci K32 kuralını hiçbir yerde uygulamaz.                                                                                                                                                                                            | RAN-02         |
| Önizleme ve override | Önizleme gövdedeki elle girilmiş `price`'ı **yansıtır**, yoksaymaz: form her değişiklikte aynı gövdeyi gönderip dönen satırları çizer, yoksa owner'ın düzeltmesi önizlemeyle silinirdi. Yoksayılan tek alan `pinPrice`'tır — önizleme hiçbir şey yazmaz. Süre her zaman kademenindir; ne özel fiyat ne elle fiyat onu değiştirir.                                                                                                                                                                              | RAN-02         |
| Son ödenen fiyat     | `lastPaidPrice`, randevunun **kendi `startAt`'ından önceki** son tamamlanmış randevunun satır fiyatıdır, bugünden önceki değil: geçmiş tarihli bir randevu girilirken (K26) "geçen sefer" o tarihten önceki ziyarettir. Sonuçlanmamış randevular sayılmaz. Yalnızca bilgidir, otomatik uygulanmaz (K32).                                                                                                                                                                                                       | RAN-02         |
| Kademe tercihi       | Hayvanın kaydındaki kademe gövdedekini **yener**: gövdenin `sizeTier`'ı yalnızca kayıt boşken sorulur ve o zaman hayvana da yazılır. Dolu bir kaydı gövdeyle geçersiz kılmak, fiyatı hayvanın kaydıyla çelişen bir randevu bırakırdı.                                                                                                                                                                                                                                                                          | RAN-02, RAN-05 |
| Kademe yazımı        | Randevu gövdesi hayvan başına opsiyonel `sizeTier` taşır. Kademesi boş hayvanda `appointments`, `PetsService` üzerinden kademeyi yazar ve randevuyu **aynı transaction'da** oluşturur; `Pet` tablosuna doğrudan dokunmaz. İstemci ayrı bir `PATCH /pets/:id` çağırmaz — ilki geçip ikincisi patlarsa kademe yazılmış, randevu yok olurdu.                                                                                                                                                                      | RAN-02, RAN-05 |
| Randevu okuma        | `GET /appointments/:id` E4'te yazılır: RAN-05'in düzenleme formu randevuyu okumak zorunda. Tam gövdeyi döner (hayvanlar `position` sırasıyla, satırlar, `note`, hesaplanmış `warnings`, iptal bilgisi). OPR-02'nin detay ekranına özel eklentileri (durum geçiş butonları, bakım raporu, döküm) E5'te gelir.                                                                                                                                                                                                   | RAN-05, OPR-02 |
| Arşivli hayvan       | Arşivlenmiş hayvanı randevuya eklemek `409 CONFLICT` alır; aynı kontrol fiyat önizlemesinde de çalışır, çünkü ikisi tek fiyat çözümü metodundan geçer. Ayrı bir hata kodu açılmadı: istemci arşivlileri chip listesinde hiç göstermiyor, yani bu koda göre dallanmıyor; hata kodu listesi yalnızca istemcinin gerçekten ayırt ettiği durumlar için büyür.                                                                                                                                                      | RAN-02, RAN-05 |
| Uyarı kodları        | `OVERLAP`, `OUTSIDE_HOURS` ve `CLOSED_DAY` `contracts/appointments.ts`'te tanımlanır, `errors.ts`'te değil: bunlar HTTP hatası değil, `APPOINTMENT_WARNINGS` gövdesindeki `errors[].code` değerleridir. `ERROR_CODES` uygulama genelindeki HTTP hatalarının listesidir ve şişirilmez.                                                                                                                                                                                                                          | RAN-04         |
| Uyarı kontrolleri    | Üç kontrol **birbirinden bağımsızdır** ve aynı gün için birden fazlası dönebilir: kapalı günün çalışma saatleri sıfırlanmadığı için (KUR-06) Pazar 20:00 hem `CLOSED_DAY` hem `OUTSIDE_HOURS` alır, haftalık kapalı gün ile özel kapalı gün aralığı da ayrı sebeplerdir. Kontrol **yerel takvime göre** yapılır: `startAt` UTC saklanır, karşılaştırma işletmenin saat diliminde olur. Gece yarısını aşan randevuda (23:00 → 01:00) iki günün de saatleri bakılır; tam gece yarısında biten randevu bittiği güne taşmaz. Aynı sebep iki kez listelenmez. | RAN-04         |
| Uyarı mesajları      | `OVERLAP` mesajı karşı randevunun yerel saat aralığını ve müşteri adını taşır ("14:00–15:30 arasındaki Ayşe Yılmaz randevusuyla çakışıyor"); sözleşmede ayrı bir `appointmentId` alanı yoktur çünkü istemci ona göre dallanmaz. `OUTSIDE_HOURS` o günün saatlerini yazar. Mesajlar hafta günü adı ve tarih **yazmaz**: takvim etiketleri istemcide durur (§6.1) ve sunucu onları kopyalamaz. | RAN-04         |
| Çakışma ikonu        | Çakışma **saklanmaz, okuma anında hesaplanır**: "çakışma, diğer randevu iptal edilir veya taşınırsa kendiliğinden kalkar" ancak hesaplanan bir alanla mümkündür. Takvim sorgusu hesabı dönen pencerenin içinde yapar; randevu gövdesinde `warnings` aynı motordan gelir.                                                                                                                                                                                                                                       | RAN-01, RAN-04 |
| Takvim aralığı       | Gün ve hafta **aynı uçtan** beslenir; aralığın genişliğini istemci belirler ve sunucuda "gün" / "hafta" kavramı yoktur. Aralık zorunludur ve üst sınırı vardır (31 gün): sınırsız aralık tek istekle bütün geçmişi çeken bir uç demektir.                                                                                                                                                                                                                                                                      | RAN-01         |
| Elle bitiş sınırı     | Gövdedeki `endAt` başlangıçtan **sonra** ve en çok `APPOINTMENT_MAX_SPAN_MINUTES` (24 saat) sonra olabilir; aksi hâlde `400`, `field: endAt`. Kural sözleşmededir, servis tekrar etmez. Bir bakım bir güne sığar (konaklama MVP'de yok) ve sınır uyarı penceresini de bağlar: motor `startAt`'tan `endAt`'a yerel gün gün ilerlediği için sınırsız bir bitiş tek istekte yüzlerce günün çalışma saatini sorardı. **Hesaplanan** bitiş (satır sürelerinin toplamı) sınıra tabi değildir. | RAN-02, RAN-05 |
| Kaydedilmiş gövde     | `POST` ve `PUT` yanıtı **saklanan satırlardan** kurulur, o istekte çözülmüş fiyatlardan değil; `GET /appointments/:id` ile aynı metottur. `priceSource` ve `lastPaidPrice` okuma anında hesaplandığı için `pinPrice` ile sabitlenen satır kendi kayıt yanıtında da `custom` görünür. İki ayrı kurucu olsa kaydedilen değerle gösterilen değer bir gün ayrışırdı. | RAN-02, RAN-05 |
| `pinPrice` ve boş fiyat | `pinPrice`, `price` boş gönderilmişse **yoksayılır**: sabitlenecek bir düzeltme yoktur ve kural fiyatı her seferinde aynı çıkar. Özel fiyat yalnızca kayıt yolunda doğar; hayvan uçlarında ekleme yok, silme var. | RAN-02, RAN-05 |
| Müşteri ve hayvan     | Gövdedeki hayvan gövdedeki müşteriye ait değilse `404` — başka işletmenin kaydı gibi yok sayılır. Müşteri önce sorulur, yani anonimleştirilmiş müşteri (K54) de `404` alır. Fiyat önizlemesi bu kontrolü yapmaz, çünkü müşteri istemez. | RAN-02, RAN-05 |
| Satır sırası          | Bakım sırası **hayvan** düzeyindedir (`AppointmentPet.position`, K30). Bir hayvanın hizmet satırları arasında saklanan sıra yoktur ve ekranda anlam taşımaz; satır başına uç geldiğinde (OPR-03) gerekirse eklenir. | RAN-02, OPR-03 |
| Düzenleme gövdesi     | `PUT /appointments/:id` gövdesi (`updateAppointmentBody`) `appointmentBody` ile **aynı alanları** alır; tek ayrıldığı yer boş hayvan listesinin mesajıdır: oluşturmada "hayvan seç", düzenlemede ise son hayvanın çıkarılmasıdır ve mesaj iptali önerir (RAN-06). İki şema ortak bir kurucudan doğar, böylece `endAt` sınırı ve "aynı hayvan bir kez" kuralı tek yerde kalır. Mesaj servise taşınamaz: boş listeyi sözleşme keser, servis o isteği hiç görmez. | RAN-05 |
| Satırların yeniden yazımı | Gövde tam olduğu için **hizmet** satırları eşleştirilmez: silinip gövdedeki hâliyle yeniden yazılır. Hangi satırın kaldığını bulmak gövdede satır `id`'si istemek demekti ve sözleşme onu taşımaz. Ek ücret satırlarına `PUT` dokunmaz (OPR-03); bu yüzden randevuda **kalan** hayvanın `appointmentPet` kaydı silinmez, `upsert` ile korunup yalnızca `position`'ı güncellenir — silinip yeniden yaratılsa ona bağlı kalem ve bakım raporu FK cascade'iyle giderdi. Gövdede artık bulunmayan hayvan silinir ve kalemi onunla birlikte gider. | RAN-05, OPR-03 |
| Müşteri değişmezliği  | `PUT` gövdesi `customerId` taşımaya devam eder ama kayıttakiyle aynı olmak zorundadır; değilse `400`, `field: customerId`. Sessizce yoksaymak formu yanıltırdı (fail fast). Müşteri `create`'teki gibi ayrıca sorulmaz: randevu zaten tenant içindedir ve müşterisi değişmiyor. | RAN-05 |
| Okuma yolunda uyarılar | `warnings` yalnızca **sonuçlanmamış** randevu için hesaplanır. Sonuçlanmış randevu düzenlenemediği için (`409`) şeridin arkasında yapılacak bir iş yoktur; ileri tarihli iptal edilmiş bir randevu için "çakışıyor" demek ise yanıltırdı — iptal edilen randevu kimsenin saatini tutmaz (K27) ve karşı taraf olarak da sayılmaz. Tamamlanmışların çoğunu K26 zaten eler. | RAN-05, OPR-02 |
| İptal ucu            | İptal ayrı bir uçtur (`POST /appointments/:id/cancel`, `200`), `PUT` ile yapılmaz: `cancelledBy` zorunludur ve işlem geri alınamaz. Düzenleme gövdesi tam olduğu için iptali oraya sığdırmak, iptal etmek için randevunun bütün hayvanlarını ve satırlarını tekrar göndermek demekti. `POST` ama yeni kayıt doğmaz, mevcut randevunun durumu yazılır — bu yüzden `201` değil `200`. Durumu geri çeviren uç **yoktur**. | RAN-06 |
| İptalde veri          | İptal bir **durum değişikliğidir**: `status`, `cancelledBy`, `cancelReason` ve `cancelledAt` yazılır, hayvanlar ve satırlar silinmez — randevu müşteri geçmişinde tutarıyla kalır (MUS-04). Zaten iptal edilmiş randevu `409 APPOINTMENT_FINALIZED` alır ve ilk iptalin bilgisi ezilmez. Müşterinin iptal sayısı saklanmaz, `cancelledBy: customer` satırlarından okuma anında hesaplanır. | RAN-06, MUS-04 |
| Türetilen alanlar     | Randevudan türeyen alanların sorguları **`appointments` modülündedir** ve çağıran orkestrasyon katmanıdır: müşterinin son ziyareti, son hizmeti ve gelmedi sayısı (`customerFacts`), detayın sayaçları ve geçmişi (`customerHistory`), hayvanın açık randevuları (`upcomingForPet`, HAY-05), kapalı güne düşen randevular (`conflictsByRange`, KUR-09). Hiçbiri `Customer`, `Pet` ya da `ClosedDay` tablosuna dokunmaz. | MUS-01, MUS-04, HAY-05, KUR-09 |
| Türetilen alan sorguları | Sorgu sayısı **satır başına değil, sayfa başına**: `customerFacts` iki `groupBy` + bir satır sorgusuyla bütün sayfayı karşılar (`(customerId, startAt)` index'i), `conflictsByRange` bütün kapalı günlerin uç aralığını tek sorguyla çekip belleğe dağıtır. Kapalı günde pencere UTC'de bir gün geniş alınır ve kesin karşılaştırma randevunun yerel günü üzerinden bellekte yapılır (§5.6); randevu **başladığı** güne sayılır, takvimdeki kuralla aynı. | MUS-01, KUR-09 |
| Ortak randevu satırı  | Müşteri geçmişi, hayvanın açık randevuları ve kapalı gün çakışmaları **aynı** `appointmentSummary` biçimini döner ve tek bir kurucudan (`summariesOf`) gelir. `durationMinutes` satır sürelerinin toplamıdır (K30) ve elle uzatılmış `endAt`'ı içermez — randevu gövdesindeki alanla aynı kural. Hayvan adı canlı veridir (`PetsService.namesByIds`), satırın adı ve fiyatı kopyalanmıştır. | MUS-04, HAY-05, KUR-09 |
| Kod yeri             | `features/appointments/` (api, queries, schema, components). Takvim ekranı `app/(tabs)/index.tsx` yalnızca kompozisyon. Web (K47) aynı API'yi ayrı bir React uygulamasından tüketir.                                                                                                                                                                                                                                                                                                                           | Tümü           |

## Diğer epic'lere bağlantılar

| Buradan        | Oraya                  | Konu                                                                 |
| -------------- | ---------------------- | -------------------------------------------------------------------- |
| RAN-01         | KUR-04, KUR-06, KUR-09 | Kurulum kartı, çalışma saatleri, kapalı günler                       |
| RAN-01         | HAY-02, HAY-03         | Takvim bloğunda uyarı ve aşı ikonları                                |
| RAN-02         | MUS-01, MUS-02         | Müşteri arama ve hızlı ekleme                                        |
| RAN-02         | HAY-01, KUR-07         | Hızlı hayvan ekleme, boyut kademesi ve fiyat                         |
| RAN-02         | INT-02                 | Yeni müşteriye "Formu gönder" önerisi                                |
| RAN-05         | HAY-05                 | Arşivlemeden önce hayvanı randevudan çıkarma                         |
| RAN-06         | MUS-04                 | Müşteri iptal sayısı                                                 |
| RAN-01, RAN-05 | E5                     | Durum akışı, randevu detayı, tamamlanmış randevu                     |
| RAN-01, RAN-02 | OPR-01                 | "Geldi mi?" sorusu: takvim bloğunda ve geçmiş tarihli yeni randevuda |
| RAN-05, RAN-06 | E6                     | Hatırlatmaların yeni saate göre çalışması ve iptalde durması         |

## Açık sorular

- Yok.

## Tasarımla uyuşmazlıklar ("Yeni Randevu" ekranı)

Story'ler kesin, tasarım güncellenecek:

- **"Görüşme aktif · Müşteri hattı 01:42"** tasarımdan çıkar. iOS üçüncü parti uygulamaya aktif aramayı göstermez; Android'de özel izin ve Play onayı gerekir. Story'lerde de yok.
- **"Masa 2 · Uygun"** çıkar. Masa veya kapasite modeli yok; RAN-04 çakışmayı yalnızca uyarı olarak ele alır.
- **Tek "Ücret" alanı** yerine hayvan ve hizmet bazında satır fiyatları, toplam altta (RAN-02).
