# E7 · Tahsilat & Kasa

| Durum     | Faz | Platform    | Bağımlılık                     |
| --------- | --- | ----------- | ------------------------------ |
| Yapılacak | MVP | Mobil + Web | [E5](05-randevu-operasyonu.md) |

> Story'ler: Kesinleşti

## Amaç

Kimin ne kadar ödediği ve ne kadar borcu kaldığı her zaman net olsun. Aylık durum tek bakışta görülsün.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [Epic Haritası](index.md#alınan-kararlar).

| #                               | Başlık                                                                    |
| ------------------------------- | ------------------------------------------------------------------------- |
| [K2](index.md#alınan-kararlar)  | "Fatura", müşteriye gönderilen hizmet dökümü ve tahsilat kaydıdır.        |
| [K37](index.md#alınan-kararlar) | Tamamlanan randevu geri alınamaz.                                         |
| [K41](index.md#alınan-kararlar) | Borç tahsilatı en eski dökümden başlayarak dökümlere otomatik dağıtılır.  |
| [K42](index.md#alınan-kararlar) | Döküm PDF olarak backend'de üretilir.                                     |
| [K43](index.md#alınan-kararlar) | İndirim yalnızca toplam tutara uygulanır; tutar veya yüzde.               |
| [K44](index.md#alınan-kararlar) | Gider kategorileri sabit liste: Malzeme, Kira, Faturalar, Ekipman, Diğer. |
| [K45](index.md#alınan-kararlar) | Fiyatlar KDV dahil; vergi hesaplanmaz.                                    |
| [K54](index.md#alınan-kararlar) | Müşteri silme tek yoldur: anonimleştirme.                                 |
| [K61](index.md#alınan-kararlar) | Açılış dökümü silinebilir; randevu dökümü silinemez.                     |
| [K62](index.md#alınan-kararlar) | Gider tarihi geleceğe yazılamaz; kasa nakit esaslıdır.                   |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

| ID     | Başlık                     | Platform    | Durum     |
| ------ | -------------------------- | ----------- | --------- |
| KAS-01 | Hizmet dökümü              | Mobil + Web | Yapılacak |
| KAS-02 | Döküm düzenleme ve indirim | Mobil + Web | Yapılacak |
| KAS-03 | Tahsilat alma              | Mobil + Web | Yapılacak |
| KAS-04 | Borç ve borç tahsili       | Mobil + Web | Yapılacak |
| KAS-05 | Ödeme kaydını silme        | Mobil + Web | Yapılacak |
| KAS-06 | Dökümü PDF olarak paylaşma | Mobil + Web | Yapılacak |
| KAS-07 | Gider kaydı                | Mobil + Web | Yapılacak |
| KAS-08 | Kasa ve aylık özet         | Mobil + Web | Yapılacak |
| KAS-09 | Alacaklar listesi          | Mobil + Web | Yapılacak |

---

## Döküm ve ödeme

**KAS-01 · Hizmet dökümü** · Mobil + Web
_Salon sahibi olarak randevu tamamlandığında hizmetlerin ve ek ücretlerin yer aldığı bir dökümün otomatik oluşmasını istiyorum._

- Döküm, randevu tamamlandığı anda (OPR-05) oluşur. İptal edilen ve "Gelmedi" olarak işaretlenen randevular için döküm oluşmaz.
- Her randevunun en fazla bir dökümü vardır.
- **Açılış borcu:** müşteri eklenirken (MUS-02) veya düzenlenirken (MUS-05) "Eski borç" alanına tutar girilirse, randevusuz bir **açılış dökümü** oluşur. Tek satırı "Açılış borcu"dur, numarası diğer dökümlerle aynı sırayı kullanır, tarihi girildiği gündür. Borç hesabı, toplu tahsilat (KAS-04) ve PDF (KAS-06) bu dökümü diğerlerinden ayırmaz. Müşteri başına en fazla bir açılış dökümü olur; düzenleme KAS-02 ile yapılır.
- **Açılış dökümü silinebilir** (K61): yanlış müşteriye ya da yanlışlıkla girilmiş açılış borcu, ödemesi yoksa dökümden silinir ve müşterinin borcundan düşer. Ödemesi varsa önce ödeme kayıtları silinir (KAS-05). Silindikten sonra "Eski borç" alanı yeniden kullanılabilir. Randevu dökümü silinemez (K37).
- Döküm içeriği:
  - İşletme içinde sıralı döküm numarası (#0042)
  - Tarih ve müşteri adı
  - Her hayvan için hizmet satırları ve ek ücretler
  - Ara toplam, indirim, toplam
  - Alınan ödemeler (tarih ve yöntemiyle) ve kalan tutar
- Döküm durumları: **Ödenmedi**, **Kısmi**, **Ödendi**. Durum saklanmaz; ödemelerden hesaplanır.
- Döküm, randevu detayındaki "Dökümü gör" butonundan (OPR-02) ve müşterinin randevu geçmişinden açılır.
- Döküm, ödeme alınmadan önce de paylaşılabilir (KAS-06); PDF o anki durumu "Ödenmedi" olarak gösterir.
  **KAS-02 · Döküm düzenleme ve indirim** · Mobil + Web
  _Salon sahibi olarak tamamlanmış bir randevunun dökümünde tutarı düzeltebilmek, kalem ekleyip çıkarabilmek ve indirim yapabilmek istiyorum._
- Satır tutarları değiştirilebilir; hizmet veya ek ücret satırı eklenip çıkarılabilir. Web'de tamamlanmış randevuya kalem eklemenin tek yolu budur (OPR-03 yalnızca Mobil).
- Dökümde en az bir satır kalmalıdır.
- İndirim toplam tutara uygulanır; tutar (₺) veya yüzde (%) olarak girilir (K43). Yüzde indirim kuruşa yuvarlanır. İndirim ara toplamı aşamaz.
- Yeni toplam, o döküm için alınmış ödemelerin toplamından düşük olamaz. Düşürmek gerekiyorsa önce ödeme kaydı silinir (KAS-05).
- Döküm düzenlemesi randevunun süresini ve saatini değiştirmez.
  **KAS-03 · Tahsilat alma** · Mobil + Web
  _Salon sahibi olarak müşteriden aldığım ödemeyi yöntemiyle birlikte kaydetmek istiyorum._
- Tahsilat ekranı, tamamlamadan sonra otomatik açılır (OPR-05) ve dökümden de açılabilir.
- Ödeme yöntemleri: **Nakit**, **Kart**, **Havale/EFT**.
- Tutar varsayılan olarak kalan tutarla dolu gelir. Daha düşük bir tutar girilirse kısmi ödeme sayılır.
- Kalan tutarı aşan ödeme girilemez.
- Ödeme tarihi varsayılan olarak bugündür ve değiştirilebilir; ileri tarih girilemez.
- Bir döküme birden fazla ödeme eklenebilir (ör. 500 ₺ nakit + 300 ₺ kart).
- Tahsilat ekranı atlanırsa döküm "Ödenmedi" kalır ve tutar müşterinin borcuna yansır.
- Randevudaki bir hayvanın aşı durumu Bilinmiyor veya Süresi geçmiş ise (HAY-03) tahsilat ekranının üstünde küçük bir uyarı görünür ("Paşa: kuduz aşısı süresi geçmiş"). Bilgi amaçlıdır, kaydı engellemez.
  **KAS-04 · Borç ve borç tahsili** · Mobil + Web
  _Salon sahibi olarak müşterinin toplam borcunu görmek ve sonradan gelen ödemeyi kolayca kaydetmek istiyorum._
- Müşterinin borcu, ödenmemiş ve kısmi ödenmiş dökümlerindeki kalan tutarların toplamıdır. Saklanmaz, hesaplanır.
- Borç şu yerlerde görünür:
  - Müşteri detayındaki borç metriği (MUS-04)
  - Randevu detayındaki müşteri kartında borç chip'i (OPR-02)
  - Müşteri listesindeki "Borçlu" filtresi (MUS-01)
  - Alacaklar listesi (KAS-09)
- Müşteri detayındaki **"Borcu tahsil et"** ile tek bir tutar ve yöntem girilir. Tutar en eski dökümden başlayarak dökümlere otomatik dağıtılır (K41).
- Girilen tutar toplam borcu aşamaz.
- Dağıtım sonucu kaydetmeden önce gösterilir ("#0038: 400 ₺ kapandı, #0042: 200 ₺ kısmi").
- Borcu olan müşteri silinemez (HES-04, K54).
  **KAS-05 · Ödeme kaydını silme** · Mobil + Web
  _Salon sahibi olarak yanlış girdiğim bir ödeme kaydını silebilmek istiyorum._
- Silmeden önce onay istenir.
- Silinen ödemenin tutarı dökümdeki kalan tutara geri eklenir; kasa ve aylık özet buna göre değişir.
- Ödeme kaydı düzenlenemez; düzeltmenin tek yolu silip yeniden eklemektir.
- Toplu tahsilatla (KAS-04) oluşmuş ödemeler de döküm bazında tek tek silinir.
  **KAS-06 · Dökümü PDF olarak paylaşma** · Mobil + Web
  _Salon sahibi olarak dökümü müşteriye PDF olarak gönderebilmek istiyorum._
- PDF backend'de üretilir (K42).
- PDF, istendiği anda dökümün güncel halinden üretilir; düzenleme veya yeni ödeme sonrası tekrar paylaşılırsa güncel hali gider.
- PDF içeriği:
  - Salon adı, telefonu ve adresi (KUR-05)
  - Döküm numarası ve tarihi
  - Müşteri adı
  - Hayvan bazında hizmet ve ek ücret satırları
  - Ara toplam, indirim, toplam, alınan ödemeler, kalan tutar
  - Alt bilgi: _"Bu belge hizmet dökümüdür, fatura yerine geçmez."_ (K45)
- Mobilde "Paylaş" butonu PDF'i indirir ve telefonun paylaşım menüsünü açar; owner WhatsApp'ı seçip gönderir.
- Web'de "İndir" butonu PDF'i indirir.
- Dosya adı döküm numarasını içerir (`Petzibu-Dokum-0042.pdf`).

---

## Gider ve kasa

**KAS-07 · Gider kaydı** · Mobil + Web
_Salon sahibi olarak giderlerimi hızlıca girebilmek istiyorum, böylece ay sonunda ne kadar harcadığımı bilirim._

- Alanlar: tutar (zorunlu), kategori (zorunlu), tarih (varsayılan bugün), isteğe bağlı not.
- Kategoriler sabit listeden seçilir (K44):
  - Malzeme (şampuan, bakım ürünleri)
  - Kira
  - Faturalar
  - Ekipman
  - Diğer
- Gider düzenlenebilir ve silinebilir; silmeden önce onay istenir.
- Tarih geleceğe yazılamaz (K62): gider paranın çıktığı gündür. Peşin ödenen kira ödendiği güne girilir, dönem bilgisi nota yazılır.
- Stok takibi yoktur.
  **KAS-08 · Kasa ve aylık özet** · Mobil + Web
  _Salon sahibi olarak bu ay ne kadar kazandığımı, ne kadar harcadığımı ve elimde ne kaldığını tek bakışta görmek istiyorum._
- Ay seçiciyle istenen ay açılır; varsayılan bu aydır.
- En üstte **"Bugün"** kartı: bugün alınan ödemelerin nakit / kart / havale toplamları ve bugün oluşan dökümlerden açık kalan borç. Seçili aydan bağımsızdır.
- Özet kartları:
  - **Gelir:** o ay alınan ödemelerin toplamı (ödeme tarihine göre)
  - **Gider:** o ayın giderlerinin toplamı
  - **Net:** gelir eksi gider
  - **Alacak:** bütün müşterilerin şu anki toplam borcu (seçili aydan bağımsız); dokununca Alacaklar listesi (KAS-09) açılır
- Gelir, ödeme yöntemlerine göre dağılımıyla gösterilir (nakit / kart / havale).
- Gider, kategori bazında dağılımıyla gösterilir.
- Altta o ayın hareketleri tarihe göre listelenir: ödemeler ve giderler. Satıra dokununca ilgili döküm veya gider açılır. Silinmiş müşterinin ödemeleri "Silinmiş müşteri" adıyla listelenir (K54).
- Sağ altta "+ Gider" butonu bulunur.
  **KAS-09 · Alacaklar listesi** · Mobil + Web
  _Salon sahibi olarak kimin bana ne kadar borcu olduğunu bir liste halinde görmek ve hatırlatmak istiyorum._
- Borcu olan müşteriler, borç tutarına göre büyükten küçüğe sıralanır.
- Her satırda müşteri adı, borç tutarı ve en eski ödenmemiş dökümün tarihi görünür.
- Satıra dokununca müşteri detayı açılır; oradan "Borcu tahsil et" kullanılabilir (KAS-04).
- Satırdaki "Hatırlat" butonu hazır bir WhatsApp mesajı açar. Mesaj "Borç hatırlatma" şablonundan üretilir (HAT-02); `{borç}` toplam borçtur.

---

## Kapsam dışı

- E-arşiv ve e-fatura, mali belge üretimi
- Online ödeme, kart saklama, ön ödeme, provizyon (Faz 3)
- İade (müşteriye para geri verme)
- Bahşiş
- Müşteri hesabında fazla ödeme veya ön ödeme bakiyesi (işaretli bakiye Faz 2'de paket/seans kartıyla birlikte değerlendirilir)
- Satır bazında indirim
- Tekrarlayan gider (ör. her ay otomatik kira)
- Gider için fiş fotoğrafı
- Hizmet bazında veya dönemsel detaylı raporlar (Faz 2, web back office)
- Gelmedi ücreti
- Dökümün metin olarak paylaşılması

## Rakip analizinden gelen kriterler

Kaynak: rakip analizi v2, §6 E7. Hepsi karara bağlandı.

- ✓ Döküm tahsilattan önce paylaşılabilir (KAS-01, KAS-06).
- ✓ Ürün satırı: KUR-08 ek ücret kalemiyle; ayrı kavram yok.
- ✓ Açılış borcu: açılış dökümü (KAS-01).
- ✓ Günlük kasa özeti: "Bugün" kartı (KAS-08).
- ✓ Tahsilat ekranında aşı uyarısı (KAS-03).
- ✗ İşaretli bakiye: kapsam dışı, Faz 2.

## Teknik notlar

| Konu                 | Karar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Story                  |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| Para                 | Tüm tutarlar kuruş cinsinden tam sayı olarak saklanır; gösterimde "1.250,00 ₺" formatı kullanılır (`lib/format.ts`).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Tümü                   |
| Döküm modeli | `statement` (`kind: appointment \| opening`, `appointmentId` benzersiz ve `kind=opening` için boş, `customerId`, `number`, `subtotal`, `discountType`, `discountValue`, `createdAt`). Randevu dökümünün satırları ayrı bir kopya değildir; tamamlanmış randevunun `appointmentLine` kayıtlarıdır. Açılış dökümünün tek satırı `statementLine` olarak kendi üzerinde durur; müşteri başına en fazla bir `opening` kaydı (kural servistedir, şemada kısıt yok). **`subtotal` saklanır** ([ADR-0010](../architecture/decisions/0010-dokum-ara-toplami-ve-statements-orkestrasyonu.md)): satırların toplamıdır ve satırları yazan transaction onu da yazar (tamamlama, KAS-02); böylece `billing` borcu kendi tablolarından hesaplar, randevu satırlarını okumak zorunda kalmaz. Tamamlanmış randevunun satırları yalnızca döküm üzerinden (KAS-02) değiştirilebilir; bu yol E4'teki `409 APPOINTMENT_FINALIZED` kısıtının tek bilinçli istisnasıdır (`AppointmentsService.rewriteCompletedLines`). Satırların tek kaynağı yine `appointmentLine`'dır. KAS-02 ile eklenen satır `durationMin` taşır ama randevunun `endAt` değeri dondurulmuştur, yeniden hesaplanmaz; hayvan listesi de değişmez, yalnızca satırlar. | KAS-01, KAS-02 |
| Modül sınırı | `billing` (domain) para kurallarının tek sahibidir ve hiçbir domain modülüne bağımlı değildir (çekirdekten yalnızca `businesses`, saat dilimi için); `Statement`, `StatementLine`, `Payment`. İki servis: döküm (`BillingService`) ve ödeme (`PaymentsService`). Controller'ı yoktur. `/statements` uçlarının tamamı **`statements` orkestrasyonundadır**: gövde `billing` (para) + `appointments` (satır içeriği) + `customers` (ad), PDF için `businesses` + `document-generator`. Yön `appointments → billing` korunur: tamamlama `billing.createForAppointment(customerId, appointmentId, subtotal)` çağırır, ara toplamı kendi satırlarından verir. Giderler `expenses` (domain), kasa özeti ve alacaklar `reports` (orkestrasyon, salt okuma). ADR-0010. | Tümü |
| KAS-02 satır kaynağı | Hizmet satırı **katalogdan** gelir (`serviceId` zorunlu, ad kopyalanır, `tier` hayvanın kademesi, fiyat serbest); ek ücret satırı **katalogsuz olabilir** (`extraChargeId` boş, ad + tutar zorunlu) — OPR-03'ün `extraChargeId`'yi nullable bırakıp KAS-02'ye işaret ettiği kapı budur. `appointmentServiceLine` ve `appointmentExtraLine` şemaları değişmez; döküm satırı randevu satırıyla aynı şemadır. Gövde tamdır (`PUT /statements/:id/lines`), en az bir satır. | KAS-02 |
| Açılış borcu | `customerBody.openingDebt` (kuruş, pozitif, opsiyonel). Oluşturmada `customer-overview` aynı transaction'da `billing.createOpening` çağırır. Güncellemede `openingDebt` dolu gelmişse yalnızca açılış dökümü **yoksa** kabul edilir; varsa `409 CONFLICT` ve düzenleme KAS-02'den (MUS-05); `openingDebt` boş gelirse mevcut döküme dokunulmaz. `409` döküm dururken geçerlidir: açılış dökümü silinince (K61, `DELETE /statements/:id`) yeniden giriş mümkündür. Açılış dökümünün satır gövdesi tek tutardır, pozitif; sıfırlama yok, kaldırma silmedir. | KAS-01, MUS-02, MUS-05 |
| Döküm numarası | İşletme bazında sıralı; tamamlama transaction'ı içinde atanır (OPR-05). Eşzamanlılık için `pg_advisory_xact_lock(hashtext(businessId))` alınır, sonra `MAX(number) + 1`; kilit transaction'la düşer. Ayrı sayaç tablosu yok, `@@unique([businessId, number])` emniyet kemeri. Raw çağrı hiçbir tabloya dokunmaz. Sözleşme tam sayı taşır; `#0042` biçimi istemcide ve PDF'te. | KAS-01 |
| Ödeme modeli | `payment` (`statementId`, `amount`, `method: cash \| card \| transfer`, `paidAt` UTC an). Silme kalıcıdır. **"İleri tarih girilemez" kuralı yerel gün düzeyindedir:** sunucu `localDateIn(paidAt, business.timezone) <= localDateIn(now, timezone)` ise kabul eder, değilse `400 VALIDATION_FAILED` (`field: paidAt`); anı karşılaştırmaz, böylece istemci saatinin birkaç saniye önde olması isteği düşürmez. İstemci kuralı: bugün seçiliyse `paidAt = şimdi`, geçmiş gün seçiliyse o günün yerel **12:00**'si — gün sınırına en uzak an; geçmiş günün ödemeleri KAS-08 hareket listesinde o günün içinde kalır.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | KAS-03, KAS-05         |
| Hesaplanan değerler | Döküm durumu, kalan tutar, müşteri borcu ve toplam alacak saklanmaz; `subtotal`, indirim ve ödemelerden hesaplanır (`subtotal` istisnası için ADR-0010). Hesap `billing`'de tek fonksiyondadır (`totalsOf`); döküm gövdesi, borç ve PDF aynı fonksiyonu kullanır. Müşteri listesi ve detayı yanıtlarında `balance` (borç) ve `totalPaid` (toplam ödeme) hazır hesaplanmış gelir (MUS-01, MUS-04); randevu detayında `customerBalance` ve `statementId` (OPR-02). | KAS-01, KAS-04, KAS-08 |
| Döküm durumu | `unpaid \| partial \| paid` saklanmaz ama **sunucu hesaplayıp yanıtta döner**; PDF aynı değeri basar, iki hesap olmaz. Öncelik: `remaining == 0 → paid` (toplamı sıfır olan döküm — %100 indirim ya da ücretsiz hizmet — ödemesiz de `paid`'dir ve borca girmez), `paid == 0 → unpaid`, aksi `partial`. (Rapor durumunun istemcide türetilmesinden [E5] farkı: burada değeri ikinci bir tüketici olan PDF de kullanıyor.) | KAS-01, KAS-06 |
| Döküm listesi | `GET /statements?customerId&open` sayfalı, dar satır (`number`, `createdAt`, `total`, `remaining`, `status`); satır içeriği taşımaz, `subtotal`'dan okur. Müşterinin dökümleri ve toplu tahsilat önizlemesi buradan; randevu detayı kendi dökümünü `statementId` ile açar (OPR-02 "Dökümü gör"). `appointmentSummary` değişmez. | KAS-01, KAS-04 |
| Toplu tahsilat | Dağıtım sunucuda tek bir işlem içinde yapılır (`POST /customers/:id/collect`); her etkilenen döküm için ayrı bir `payment` kaydı oluşur, en eski `createdAt` önce (K41). Dağıtım kuralı `billing`'de saf fonksiyondur (`allocate`), birim testli. Önizleme istemcide, müşterinin açık dökümlerinden (`GET /statements?customerId&open=true`) aynı kuralla hesaplanır; sunucu tek doğruluk kaynağıdır. | KAS-04 |
| Tutarlılık kuralları | Duruma bağlı çakışmalar adlı `409`'dur: toplamın alınan ödemenin altına düşmesi (KAS-02) `409 STATEMENT_BELOW_PAID`, kalanı aşan ödeme ve toplam borcu aşan toplu tahsilat (KAS-03, KAS-04) `409 PAYMENT_EXCEEDS_REMAINING`, ödemesi olan açılış dökümünü silme (K61) `409 STATEMENT_HAS_PAYMENTS`; randevu dökümünü silme denemesi `409 APPOINTMENT_FINALIZED`. Girdi hataları BS3'ün kurduğu desenle `400 VALIDATION_FAILED` + `field`: ara toplamı aşan indirim (`discountValue`), gelecekteki `paidAt`, boş satır listesi. (Eski "422 VALIDATION_FAILED" ifadesi BS5 hazırlığında düzeltildi; `422` yalnızca `APPOINTMENT_WARNINGS`'tir.) | KAS-02, KAS-03, KAS-04 |
| PDF | Mevcut PDF adapter ile backend'de, istek anında üretilir (`GET /statements/:id/pdf`). Türkçe karakterler için font gömülür. Adapter `statements` modülünde (`@RegisterPdfAdapter`), şablon verisi döküm gövdesinin kendisidir + salon bilgisi (KUR-05). Puppeteer kapalıysa `503 PDF_UNAVAILABLE`. **e2e şablon motorunu taklit eder** (`TemplateEngineService`, S3 deseni): içerik türü, dosya adı ve şablon verisi doğrulanır, gerçek render yerelde Chrome ile; CI'a Chrome kurulmaz (prod imajı ve CI'da render Faz 5). Mobilde dosya indirilip `expo-sharing` ile paylaşılır (tek dosya, tam uyar). | KAS-06 |
| Dosya indirme        | PDF yetkili bir uç noktadan gelir; `lib/api.ts`'e `downloadFile()` yardımcısı eklenir ki token ekleme ve 401/403 işleme tek yerde kalsın. HES-05 dışa aktarma aynı yardımcıyı kullanır. Doğrudan `fetch` veya `FileSystem.downloadAsync` çağrısı feature kodunda olmaz.                                                                                                                                                                                                                                                                                                                                                                                                                                                       | KAS-06                 |
| Bugün kartı | Aylık özetle aynı uç nokta (`GET /reports/summary`), `dateFrom`/`dateTo` bugün. Ayrı model yok. "Bugün oluşan dökümlerden açık kalan borç" için `openedToday` alanı: aralıkta oluşan dökümlerin kalanlarının toplamı; ay görünümünde de döner, istemci yalnızca bugün kartında gösterir. "Alacak" aralıktan bağımsızdır. | KAS-08 |
| Aşı uyarısı          | Tahsilat ekranı OPR-02 yanıtındaki hayvan aşı durumlarını kullanır; ek istek yok.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | KAS-03                 |
| Borç mesajı | KAS-09 mesaj metnini backend üretir (HAT-02 şablon servisi, `GET /customers/:id/debt-message`); istemci şablon çözmez. **E6'ya bırakıldı** (BS5 hazırlığı): uç BS6'da şablon servisiyle gelir, koda gömülü ikinci bir şablon açılmaz (`reportMessage` tek kalıntı kalır). Alacaklar listesi E7'de, "Hatırlat" butonu uç gelene kadar gizli. | KAS-09 |
| Gider tarihi | `Expense.date` yerel gün; **gelecek gün `400 VALIDATION_FAILED`** (`field: date`), kural ödemeyle aynı: `date <= localDateIn(now, business.timezone)` (K62). Oluşturma ve güncellemede aynı kontrol. | KAS-07 |
| Ay sınırları | Aylık özet Europe/Istanbul saat dilimine göre hesaplanır; `dateFrom`/`dateTo` sunucuya UTC gönderilir (randevu listesiyle aynı biçim, 31 gün sınırı). Ödemeler `paidAt` (an) ile, giderler `date` (yerel gün) ile süzülür; aralık `localDateIn` ile yerel güne çevrilir ve iki ekseni birleştiren tek yer `reports`'tur. Gider listesi (`GET /expenses`) ise doğrudan yerel gün string'leri alır, çünkü gider güne yazılır. | KAS-07, KAS-08 |
| Kod yeri             | `features/finance/` (statements, payments, expenses, summary). Kasa sekmesi `app/(tabs)/cashbox.tsx` yalnızca kompozisyon.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Tümü                   |

## Diğer epic'lere bağlantılar

| Buradan        | Oraya                  | Konu                                                          |
| -------------- | ---------------------- | ------------------------------------------------------------- |
| KAS-01         | OPR-05                 | Tamamlamada döküm oluşması ve tahsilat ekranı                 |
| KAS-01, KAS-02 | OPR-03, KUR-08         | Ek ücret satırları; web'de tamamlanmış randevuya kalem ekleme |
| KAS-04         | MUS-01, MUS-04, OPR-02 | Borç gösterimi ve "Borçlu" filtresi                           |
| KAS-04         | HES-04                 | Borçlu müşteri silinemez                                      |
| KAS-01         | MUS-02, MUS-05         | "Eski borç" alanı açılış dökümü oluşturur                     |
| KAS-03         | HAY-03, OPR-02         | Tahsilat ekranında aşı uyarısı                                |
| KAS-06         | KUR-05                 | Salon bilgilerinin PDF'te kullanılması                        |
| KAS-06         | HES-05                 | `downloadFile()` yardımcısı ortak                             |
| KAS-09         | HAT-02                 | Borç hatırlatma mesaj şablonu                                 |
| KAS-08         | HES-05                 | Veri dışa aktarma (kasa sayfası)                              |

## Açık sorular

- Yok.
