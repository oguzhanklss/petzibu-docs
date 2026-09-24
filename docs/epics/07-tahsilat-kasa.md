# E7 · Tahsilat & Kasa

| Durum | Faz | Platform | Bağımlılık |
| --- | --- | --- | --- |
| Yapılacak | MVP | Mobil + Web | [E5](05-randevu-operasyonu.md) |

> Story'ler: Kesinleşti v1.2 · 24 Eylül 2026

## Amaç

Kimin ne kadar ödediği ve ne kadar borcu kaldığı her zaman net olsun. Aylık durum tek bakışta görülsün.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [README](index.md#alınan-kararlar).

| # | Karar |
| --- | --- |
| K2 | "Fatura" yasal fatura değil; **hizmet dökümü ve tahsilat kaydıdır**. Gelir, tamamlama anında değil **ödeme alındığında** oluşur. |
| K37 | Tamamlanan randevu geri alınamaz; düzeltmeler döküm üzerinden yapılır. |
| K41 | Borç tahsilatı, **en eski dökümden başlayarak** dökümlere otomatik dağıtılır. |
| K42 | Döküm **PDF olarak backend'de** üretilir (mevcut PDF adapter). Mobilde paylaşım menüsüyle, web'de indirme ile paylaşılır. Ayrı bir metin versiyonu yoktur. |
| K43 | İndirim yalnızca **toplam tutara** uygulanır; tutar veya yüzde olarak girilir. |
| K44 | Gider kategorileri **sabit listedir**: Malzeme, Kira, Faturalar, Ekipman, Diğer. |
| K45 | Fiyatlar KDV dahildir; vergi hesaplanmaz. Döküm mali belge değildir ve bunu üzerinde belirtir. |
| K54 | Silinen (anonimleştirilen) müşterinin dökümleri ve ödemeleri kalır; borçlu müşteri silinemez. |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

| ID | Başlık | Platform | Durum |
| --- | --- | --- | --- |
| KAS-01 | Hizmet dökümü | Mobil + Web | Yapılacak |
| KAS-02 | Döküm düzenleme ve indirim | Mobil + Web | Yapılacak |
| KAS-03 | Tahsilat alma | Mobil + Web | Yapılacak |
| KAS-04 | Borç ve borç tahsili | Mobil + Web | Yapılacak |
| KAS-05 | Ödeme kaydını silme | Mobil + Web | Yapılacak |
| KAS-06 | Dökümü PDF olarak paylaşma | Mobil + Web | Yapılacak |
| KAS-07 | Gider kaydı | Mobil + Web | Yapılacak |
| KAS-08 | Kasa ve aylık özet | Mobil + Web | Yapılacak |
| KAS-09 | Alacaklar listesi | Mobil + Web | Yapılacak |

---

## Döküm ve ödeme

**KAS-01 · Hizmet dökümü** · Mobil + Web
*Salon sahibi olarak randevu tamamlandığında hizmetlerin ve ek ücretlerin yer aldığı bir dökümün otomatik oluşmasını istiyorum.*

- Döküm, randevu tamamlandığı anda (OPR-05) oluşur. İptal edilen ve "Gelmedi" olarak işaretlenen randevular için döküm oluşmaz.
- Her randevunun en fazla bir dökümü vardır.
- **Açılış borcu:** müşteri eklenirken (MUS-02) veya düzenlenirken (MUS-05) "Eski borç" alanına tutar girilirse, randevusuz bir **açılış dökümü** oluşur. Tek satırı "Açılış borcu"dur, numarası diğer dökümlerle aynı sırayı kullanır, tarihi girildiği gündür. Borç hesabı, toplu tahsilat (KAS-04) ve PDF (KAS-06) bu dökümü diğerlerinden ayırmaz. Müşteri başına en fazla bir açılış dökümü olur; düzenleme KAS-02 ile yapılır.
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
*Salon sahibi olarak tamamlanmış bir randevunun dökümünde tutarı düzeltebilmek, kalem ekleyip çıkarabilmek ve indirim yapabilmek istiyorum.*
- Satır tutarları değiştirilebilir; hizmet veya ek ücret satırı eklenip çıkarılabilir. Web'de tamamlanmış randevuya kalem eklemenin tek yolu budur (OPR-03 yalnızca Mobil).
- Dökümde en az bir satır kalmalıdır.
- İndirim toplam tutara uygulanır; tutar (₺) veya yüzde (%) olarak girilir (K43). Yüzde indirim kuruşa yuvarlanır. İndirim ara toplamı aşamaz.
- Yeni toplam, o döküm için alınmış ödemelerin toplamından düşük olamaz. Düşürmek gerekiyorsa önce ödeme kaydı silinir (KAS-05).
- Döküm düzenlemesi randevunun süresini ve saatini değiştirmez.
**KAS-03 · Tahsilat alma** · Mobil + Web
*Salon sahibi olarak müşteriden aldığım ödemeyi yöntemiyle birlikte kaydetmek istiyorum.*
- Tahsilat ekranı, tamamlamadan sonra otomatik açılır (OPR-05) ve dökümden de açılabilir.
- Ödeme yöntemleri: **Nakit**, **Kart**, **Havale/EFT**.
- Tutar varsayılan olarak kalan tutarla dolu gelir. Daha düşük bir tutar girilirse kısmi ödeme sayılır.
- Kalan tutarı aşan ödeme girilemez.
- Ödeme tarihi varsayılan olarak bugündür ve değiştirilebilir; ileri tarih girilemez.
- Bir döküme birden fazla ödeme eklenebilir (ör. 500 ₺ nakit + 300 ₺ kart).
- Tahsilat ekranı atlanırsa döküm "Ödenmedi" kalır ve tutar müşterinin borcuna yansır.
- Randevudaki bir hayvanın aşı durumu Bilinmiyor veya Süresi geçmiş ise (HAY-03) tahsilat ekranının üstünde küçük bir uyarı görünür ("Paşa: kuduz aşısı süresi geçmiş"). Bilgi amaçlıdır, kaydı engellemez.
**KAS-04 · Borç ve borç tahsili** · Mobil + Web
*Salon sahibi olarak müşterinin toplam borcunu görmek ve sonradan gelen ödemeyi kolayca kaydetmek istiyorum.*
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
*Salon sahibi olarak yanlış girdiğim bir ödeme kaydını silebilmek istiyorum.*
- Silmeden önce onay istenir.
- Silinen ödemenin tutarı dökümdeki kalan tutara geri eklenir; kasa ve aylık özet buna göre değişir.
- Ödeme kaydı düzenlenemez; düzeltmenin tek yolu silip yeniden eklemektir.
- Toplu tahsilatla (KAS-04) oluşmuş ödemeler de döküm bazında tek tek silinir.
**KAS-06 · Dökümü PDF olarak paylaşma** · Mobil + Web
*Salon sahibi olarak dökümü müşteriye PDF olarak gönderebilmek istiyorum.*
- PDF backend'de üretilir (K42).
- PDF, istendiği anda dökümün güncel halinden üretilir; düzenleme veya yeni ödeme sonrası tekrar paylaşılırsa güncel hali gider.
- PDF içeriği:
  - Salon adı, telefonu ve adresi (KUR-05)
  - Döküm numarası ve tarihi
  - Müşteri adı
  - Hayvan bazında hizmet ve ek ücret satırları
  - Ara toplam, indirim, toplam, alınan ödemeler, kalan tutar
  - Alt bilgi: *"Bu belge hizmet dökümüdür, fatura yerine geçmez."* (K45)
- Mobilde "Paylaş" butonu PDF'i indirir ve telefonun paylaşım menüsünü açar; owner WhatsApp'ı seçip gönderir.
- Web'de "İndir" butonu PDF'i indirir.
- Dosya adı döküm numarasını içerir (`Petzibu-Dokum-0042.pdf`).

---

## Gider ve kasa

**KAS-07 · Gider kaydı** · Mobil + Web
*Salon sahibi olarak giderlerimi hızlıca girebilmek istiyorum, böylece ay sonunda ne kadar harcadığımı bilirim.*

- Alanlar: tutar (zorunlu), kategori (zorunlu), tarih (varsayılan bugün), isteğe bağlı not.
- Kategoriler sabit listeden seçilir (K44):
  - Malzeme (şampuan, bakım ürünleri)
  - Kira
  - Faturalar
  - Ekipman
  - Diğer
- Gider düzenlenebilir ve silinebilir; silmeden önce onay istenir.
- Stok takibi yoktur.
**KAS-08 · Kasa ve aylık özet** · Mobil + Web
*Salon sahibi olarak bu ay ne kadar kazandığımı, ne kadar harcadığımı ve elimde ne kaldığını tek bakışta görmek istiyorum.*
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
*Salon sahibi olarak kimin bana ne kadar borcu olduğunu bir liste halinde görmek ve hatırlatmak istiyorum.*
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

| Konu | Karar | Story |
| --- | --- | --- |
| Para | Tüm tutarlar kuruş cinsinden tam sayı olarak saklanır; gösterimde "1.250,00 ₺" formatı kullanılır (`lib/format.ts`). | Tümü |
| Döküm modeli | `statement` (`kind: appointment \| opening`, `appointmentId` benzersiz ve `kind=opening` için boş, `customerId`, `number`, `discountType`, `discountValue`, `createdAt`). Randevu dökümünün satırları ayrı bir kopya değildir; tamamlanmış randevunun `appointmentLine` kayıtlarıdır. Açılış dökümünün tek satırı `statementLine` olarak kendi üzerinde durur; müşteri başına en fazla bir `opening` kaydı. Tamamlanmış randevunun satırları yalnızca döküm üzerinden (KAS-02) değiştirilebilir; bu uç nokta E4'teki `409 APPOINTMENT_FINALIZED` kısıtından ayrıdır. Böylece satırların tek bir kaynağı olur. KAS-02 ile eklenen satır `durationMin` taşır ama randevunun `endAt` değeri dondurulmuştur, yeniden hesaplanmaz. | KAS-01, KAS-02 |
| Döküm numarası | İşletme bazında sıralı; eşzamanlı tamamlamalarda çakışmaması için işlem içinde atanır. Tamamlama ve döküm tek transaction (OPR-05). | KAS-01 |
| Ödeme modeli | `payment` (`statementId`, `amount`, `method: cash \| card \| transfer`, `paidAt`). Silme kalıcıdır. `paidAt` gelecekte olamaz. | KAS-03, KAS-05 |
| Hesaplanan değerler | Döküm durumu, kalan tutar, müşteri borcu ve toplam alacak saklanmaz; ödemelerden ve satırlardan hesaplanır. Müşteri listesi ve detayı yanıtlarında `balance` (borç) ve `totalPaid` (toplam ödeme) hazır hesaplanmış gelir (MUS-01, MUS-04). | KAS-01, KAS-04, KAS-08 |
| Toplu tahsilat | Dağıtım sunucuda tek bir işlem içinde yapılır; her etkilenen döküm için ayrı bir `payment` kaydı oluşur. Önizleme istemcide, müşterinin açık dökümlerinden aynı kuralla hesaplanır; sunucu tek doğruluk kaynağıdır. | KAS-04 |
| Tutarlılık kuralları | Toplamın alınan ödemenin altına düşmesi, kalanı aşan ödeme ve ara toplamı aşan indirim API'de `422 VALIDATION_FAILED` ile reddedilir. | KAS-02, KAS-03 |
| PDF | Mevcut PDF adapter ile backend'de, istek anında üretilir (`GET /statements/:id/pdf`). Türkçe karakterler için font gömülür. Mobilde dosya indirilip `expo-sharing` ile paylaşılır (tek dosya, tam uyar). | KAS-06 |
| Dosya indirme | PDF yetkili bir uç noktadan gelir; `lib/api.ts`'e `downloadFile()` yardımcısı eklenir ki token ekleme ve 401/403 işleme tek yerde kalsın. HES-05 dışa aktarma aynı yardımcıyı kullanır. Doğrudan `fetch` veya `FileSystem.downloadAsync` çağrısı feature kodunda olmaz. | KAS-06 |
| Bugün kartı | Aylık özetle aynı uç nokta, `dateFrom`/`dateTo` bugün. Ayrı model yok. | KAS-08 |
| Aşı uyarısı | Tahsilat ekranı OPR-02 yanıtındaki hayvan aşı durumlarını kullanır; ek istek yok. | KAS-03 |
| Borç mesajı | KAS-09 mesaj metnini backend üretir (HAT-02 şablon servisi, `GET /customers/:id/debt-message`); istemci şablon çözmez. | KAS-09 |
| Ay sınırları | Aylık özet Europe/Istanbul saat dilimine göre hesaplanır; `dateFrom`/`dateTo` sunucuya UTC gönderilir. | KAS-08 |
| Kod yeri | `features/finance/` (statements, payments, expenses, summary). Kasa sekmesi `app/(tabs)/cashbox.tsx` yalnızca kompozisyon. | Tümü |

## Diğer epic'lere bağlantılar

| Buradan | Oraya | Konu |
| --- | --- | --- |
| KAS-01 | OPR-05 | Tamamlamada döküm oluşması ve tahsilat ekranı |
| KAS-01, KAS-02 | OPR-03, KUR-08 | Ek ücret satırları; web'de tamamlanmış randevuya kalem ekleme |
| KAS-04 | MUS-01, MUS-04, OPR-02 | Borç gösterimi ve "Borçlu" filtresi |
| KAS-04 | HES-04 | Borçlu müşteri silinemez |
| KAS-01 | MUS-02, MUS-05 | "Eski borç" alanı açılış dökümü oluşturur |
| KAS-03 | HAY-03, OPR-02 | Tahsilat ekranında aşı uyarısı |
| KAS-06 | KUR-05 | Salon bilgilerinin PDF'te kullanılması |
| KAS-06 | HES-05 | `downloadFile()` yardımcısı ortak |
| KAS-09 | HAT-02 | Borç hatırlatma mesaj şablonu |
| KAS-08 | HES-05 | Veri dışa aktarma (kasa sayfası) |

## Açık sorular

- Yok.
