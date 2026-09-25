# E8 · Hesap & Veri

| Durum     | Faz | Platform    | Bağımlılık                  |
| --------- | --- | ----------- | --------------------------- |
| Yapılacak | MVP | Mobil + Web | [E1](01-kurulum-isletme.md) |

> Story'ler: Kesinleşti

## Amaç

Güven vermek ve yasal gerekliliklere uymak. Owner istediği an verisini alabilsin, istediği an gidebilsin; bunu bilmek kalmasını kolaylaştırır.

## Bu epic'i etkileyen kararlar

Tam liste için bkz. [Epic Haritası](index.md#alınan-kararlar).

| # | Başlık |
| --- | --- |
| [K10](index.md#alınan-kararlar) | Ödeme uygulama dışında alınır. |
| [K20](index.md#alınan-kararlar) | Hayvan silinmez, arşivlenir. |
| [K34](index.md#alınan-kararlar) | Ödeme gecikince 7 gün Ödeme bekliyor, sonra Askıda. |
| [K35](index.md#alınan-kararlar) | KVKK rolleri: owner verisinde veri sorumlusu Petzibu; müşteri ve hayvan verisinde salon, Petzibu veri işleyen. |
| [K46](index.md#alınan-kararlar) | Push bildirimi MVP'de yok. |
| [K54](index.md#alınan-kararlar) | Müşteri silme tek yoldur: anonimleştirme. |
| [K55](index.md#alınan-kararlar) | Dışa aktarma tek Excel dosyasıdır, dört sayfa: Müşteriler, Hayvanlar, Randevular, Kasa. |
| [K56](index.md#alınan-kararlar) | Tema, dil ve bildirim ayarı yok. |
| [K57](index.md#alınan-kararlar) | Hesap silme şifreyle onaylanır; işletme Silinecek durumuna geçer, 30 gün içinde vazgeçilebilir. |
| [K58](index.md#alınan-kararlar) | Owner metinlerinin yeni sürümü yayınlanınca owner bir sonraki girişte yeniden onaylar. |

## Story listesi

İlerleme bu tablodan takip edilir. Durum: `Yapılacak` → `Devam ediyor` → `Tamamlandı`.

| ID     | Başlık                                | Platform          | Durum     |
| ------ | ------------------------------------- | ----------------- | --------- |
| ADM-03 | Metin sürümü yayınlama                | İç araç (backend) | Yapılacak |
| HES-01 | Ayarlar ekranı ve hesap bilgileri     | Mobil + Web       | Yapılacak |
| HES-02 | Çıkış yapma                           | Mobil + Web       | Yapılacak |
| HES-03 | KVKK ve sözleşme metinleri            | Mobil + Web       | Yapılacak |
| HES-04 | Müşteri silme                         | Mobil + Web       | Yapılacak |
| HES-05 | Veri dışa aktarma                     | Mobil + Web       | Yapılacak |
| HES-06 | Abonelik iptal talebi                 | Mobil + Web       | Yapılacak |
| HES-07 | Hesap silme                           | Mobil + Web       | Yapılacak |

---

## İç araçlar (Petzibu ekibi)

**ADM-03 · Metin sürümü yayınlama** · İç araç
_Petzibu ekibi olarak KVKK ve sözleşme metinlerinin yeni sürümünü yayınlayabilmek istiyorum, böylece hangi owner'ın ve müşterinin hangi metni ne zaman onayladığı her zaman bilinir._

- Beş metin türü vardır (K35):
  1. **Owner aydınlatma metni** — veri sorumlusu Petzibu
  2. **Kullanım koşulları**
  3. **Veri işleme sözleşmesi** — salon veri sorumlusu, Petzibu veri işleyen
  4. **Müşteri aydınlatma metni** — işletme bazlı şablon; `{salon}` ve `{salon_adres}` yer tutucuları (INT-03)
  5. **Bakım riski onayı** — INT-03'teki onay kutusunun metni
- Her yayın yeni bir sürüm numarası alır; eski sürümler silinmez.
- Owner metinlerinin (1–3) yeni sürümü, bütün owner'ların bir sonraki girişinde yeniden onay ister (HES-03, K58).
- Müşteri metinlerinin (4–5) yeni sürümü yalnızca bundan sonraki intake formlarında gösterilir (K58).
- MVP'de bir yönetim paneli değil; ADM-01 ile aynı iç araç.

---

## Ayarlar

**HES-01 · Ayarlar ekranı ve hesap bilgileri** · Mobil + Web
_Salon sahibi olarak hesabımla ilgili her şeyi tek bir ayarlar ekranında bulmak istiyorum._

- Ayarlar ekranının bölümleri, bu sırayla:
  1. **İşletme:** işletme bilgileri (KUR-05), çalışma saatleri (KUR-06), hizmetler (KUR-07), ek ücretler (KUR-08), kapalı günler (KUR-09), form linki (INT-01)
  2. **Hatırlatmalar:** zamanlar (HAT-01), şablonlar (HAT-02)
  3. **Hesap:** owner adı ve e-postası (görüntüleme), şifre değiştirme, çıkış (HES-02)
  4. **Veri ve gizlilik:** sözleşmeler ve metinler (HES-03), verimi dışa aktar (HES-05)
  5. **Abonelik:** işletme durumu (Pilot / Aktif), iptal talebi (HES-06)
  6. **Tehlikeli bölge:** hesabı sil (HES-07)
- Owner adı ve e-postası uygulamadan değiştirilemez; değişiklik Petzibu ekibinden istenir.
- **Şifre değiştirme:** mevcut şifre, yeni şifre (en az 8 karakter). Şifre değişince diğer cihazlardaki oturumlar kapanır (KUR-03 ile aynı kural).
- Tema, dil ve bildirim ayarı yoktur (K56).
- Askıda (K34) ve Silinecek (K57) durumlarında ekranın üstünde ilgili şerit görünür; yazma gerektiren satırlar pasiftir, dışa aktarma ve KVKK işlemleri çalışır.
**HES-02 · Çıkış yapma** · Mobil + Web
_Salon sahibi olarak bu cihazdaki oturumu kapatabilmek istiyorum._

- "Çıkış yap" onay ister.
- Yalnızca bu cihazın oturumu kapanır; diğer cihazlar etkilenmez (KUR-02).
- Çıkışta cihazdaki token ve önbellek temizlenir; giriş ekranı açılır.
**HES-03 · KVKK ve sözleşme metinleri** · Mobil + Web
_Salon sahibi olarak onayladığım metinleri istediğim zaman okuyabilmek, yeni sürüm çıktığında haberdar olup onaylayabilmek istiyorum._

- "Sözleşmeler ve metinler" ekranında üç owner metni (ADM-03'teki 1–3) ve iki müşteri metni (4–5) listelenir. Her satırda güncel sürüm ve owner metinleri için "onaylandı: 12 Eyl 2026" bilgisi.
- Müşteri metinleri owner tarafından düzenlenemez; salon adı ve adresi dolu haliyle önizlenir. Owner, müşterisine göstermek isterse buradan paylaşabilir.
- **Yeniden onay (K58):** owner metinlerinden birinin yeni sürümü varsa girişte tam ekran bir onay adımı çıkar: değişen metinler linkle gösterilir, tek onay kutusu, "Devam" butonu. Onaylamadan uygulama kullanılamaz; çıkış yapılabilir.
- Her onay, metin türü, sürüm ve zamanla saklanır. Aynı yapı INT-03/INT-05'teki müşteri onayları için de kullanılır.

---

## Veri

**HES-04 · Müşteri silme** · Mobil + Web
_Salon sahibi olarak "beni silin" diyen müşterinin kişisel verisini silebilmek istiyorum, böylece KVKK talebini karşılarım ama kasa geçmişim bozulmaz._

- Müşteri detayındaki "⋯" menüsünde "Müşteriyi sil". Onay ekranı nelerin silineceğini ve nelerin kalacağını açıkça yazar.
- Silme aslında **anonimleştirmedir** (K54); arayüzde "sil" denir, "anonimleştir" kelimesi kullanılmaz.
- Silinenler: ad, telefon, etiketler, not, KVKK ve bakım riski onay kayıtları, bekleyen intake formları, kişiye özel intake linkleri, `rebookReminderAt`; hayvanların adı, ırkı, notları, uyarı etiketleri, aşı kayıtları, özel fiyatları, fotoğrafları (referans dahil) ve bakım raporları.
- Kalanlar: geçmiş randevular (tarih, satırlar, tutar, durum), dökümler, ödemeler, giderler. Bunlar "Silinmiş müşteri" ve "Silinmiş hayvan" adıyla görünür; kasa ve aylık özet (KAS-08) değişmez.
- Engeller: müşterinin **borcu** varsa veya **ileri tarihli sonuçlanmamış randevusu** varsa silinemez. Ekran borcu (KAS-04) ya da randevu listesini (HAY-05 ile aynı desen) gösterir; owner önce onları çözer.
- Silinen müşteri listede, aramada, gelmeyenler listesinde ve kuyruklarda görünmez. Aynı telefon numarasıyla yeniden müşteri eklenebilir.
- Silme geri alınamaz.
- Askıda ve Silinecek durumlarında da çalışır (K34).
**HES-05 · Veri dışa aktarma** · Mobil + Web
_Salon sahibi olarak bütün verimi tek dosyada alabilmek istiyorum, böylece Petzibu'ya bağımlı hissetmem._

- "Verimi dışa aktar" butonu tek bir Excel dosyası üretir (K55). Dört sayfa:
  - **Müşteriler:** ad, telefon, etiketler, not, eklenme tarihi, son ziyaret, toplam ödeme, borç
  - **Hayvanlar:** müşteri, ad, tür, ırk, kilo, boyut, doğum yılı, uyarı etiketleri, kuduz ve karma aşı tarihleri, arşiv durumu
  - **Randevular:** tarih, saat, müşteri, hayvanlar, hizmetler, toplam, durum, iptal eden
  - **Kasa:** tarih, tür (ödeme / gider), müşteri veya kategori, yöntem, tutar, döküm numarası
- Fotoğraflar dosyaya girmez.
- Silinmiş müşteriler "Silinmiş müşteri" olarak yer alır.
- Mobilde dosya indirilir ve paylaşım menüsü açılır; web'de indirilir. Dosya adı tarih içerir (`Petzibu-Veri-2026-09-24.xlsx`).
- Askıda ve Silinecek durumlarında çalışır (K34, K57).
- Üretim birkaç saniye sürebilir; buton işlem sırasında pasiftir.

---

## Abonelik ve hesap

**HES-06 · Abonelik iptal talebi** · Mobil + Web
_Salon sahibi olarak aboneliğimi bitirmek istediğimde ne yapacağımı bilmek istiyorum._

- Abonelik bölümünde işletme durumu görünür: "Pilot" veya "Aktif". Ödeme bekliyor ve Askıda durumlarında ADM-02'deki şerit ve metin geçerlidir.
- "Aboneliği iptal et" butonu, K10 gereği uygulama içinde bir işlem yapmaz; Petzibu ile WhatsApp veya e-posta iletişimini hazır bir mesajla açar ("Merhaba, {salon} için aboneliği iptal etmek istiyorum").
- Mobilde fiyat, ödeme veya "abone ol" ifadesi yoktur (K10). Web'de "Ödeme bilgileri" sayfası bağlantısı bulunur (K34).
- İptal sonrası hesap Petzibu ekibi tarafından Askıda'ya alınır (ADM-02); veri erişimi K34 kurallarıyla sürer. Bu, ekranda bir cümleyle anlatılır: "İptal sonrası verilerine 12 ay boyunca erişebilirsin."
**HES-07 · Hesap silme** · Mobil + Web
_Salon sahibi olarak hesabımı ve bütün verimi silebilmek istiyorum._

- "Hesabı sil" ekranı önce dışa aktarmayı önerir (HES-05), sonra nelerin silineceğini listeler: işletme, owner hesabı, bütün müşteri, hayvan, randevu, döküm, ödeme, gider ve fotoğraflar.
- Onay için owner **şifresini** girer (K57).
- Onaylanınca işletme **Silinecek** durumuna geçer: 30 gün boyunca salt okunur (Askıda ile aynı mekanizma). Owner'a e-posta gönderilir.
- Bu sürede giriş yapılabilir; üstte "Hesabın 18 gün sonra silinecek" şeridi ve **"Vazgeç"** butonu görünür. Vazgeçilince işletme önceki durumuna döner.
- 30 gün dolunca ADM-02'deki günlük job her şeyi kalıcı siler; owner'a son e-posta gider. Bu, K34'teki 12 ay sonrası silmeyle aynı job ve aynı silme yoludur.
- Silme sırasında intake linkleri ve onay linkleri geçersiz olur.
- App Store ve Play Store gerekliliği bu story ile karşılanır; mağaza listesinde hesap silme linki olarak ayarlar ekranı gösterilir.

---

## Kapsam dışı

- Tema, dil ve bildirim ayarları (K56)
- Owner adı ve e-postasını uygulamadan değiştirme
- Müşteriyi randevu ve kasa geçmişiyle birlikte tam silme (K54)
- Seçmeli veya filtreli dışa aktarma; fotoğraf dışa aktarma
- Excel'den içe aktarma (Faz 2, web)
- Uygulama içi ödeme, abonelik yönetimi, fatura görüntüleme (K10)
- Personel ve çoklu kullanıcı (Faz 2)
- Hesap silmeyi anında yapma
- Müşterinin kendi verisini kendisinin silmesi veya görmesi (pet sahibi uygulaması yok)

## Teknik notlar

| Konu | Karar | Story |
| --- | --- | --- |
| Metin modeli | `legalText` (`type: owner_privacy \| terms \| dpa \| customer_privacy \| grooming_risk`, `version`, `body`, `publishedAt`). Sürüm tam sayı, tür başına artan. Müşteri metinleri `{salon}` ve `{salon_adres}` yer tutucularını backend'de doldurur (HAT-02'deki mesaj üretimiyle aynı yaklaşım). | ADM-03, HES-03 |
| Onay modeli | Tek tablo `consent` (`subjectType: user \| customer`, `subjectId`, `textType`, `version`, `acceptedAt`). KUR-01, INT-03/INT-05 ve HES-03 hepsi buraya yazar. Eksik onay kontrolü: kullanıcının her owner metni için `consent.version == legalText.version` olmalı; değilse `GET /me` yanıtında `pendingConsents[]` döner ve istemci onay adımını açar. | HES-03 |
| Anonimleştirme | Tek transaction: `customer.name = "Silinmiş müşteri"`, `phone = null`, `anonymizedAt` dolar; `tags`, `note`, `rebookReminderAt` boş. Hayvanlar: `name = "Silinmiş hayvan"`, diğer alanlar null, `archivedAt` dolar; `petServicePrice`, `vaccination`, `photo`, `groomingReport`, `consent`, `intakeLink`, `intakeSubmission` kayıtları ve depolamadaki dosyalar silinir. `appointment`, `appointmentPet`, `appointmentLine`, `statement`, `payment` dokunulmaz. Telefon benzersizliği `phone IS NOT NULL` üzerinden çalışır. Borç veya ileri tarihli randevu varsa `409 CUSTOMER_HAS_BALANCE` / `409 CUSTOMER_HAS_UPCOMING_APPOINTMENTS`. | HES-04 |
| Anonim müşteri süzgeci | Liste, arama, gelmeyenler ve kuyruk sorguları `anonymizedAt IS NULL` süzer. Randevu ve döküm yanıtları müşteri adını olduğu gibi döner ("Silinmiş müşteri"). | HES-04 |
| Dışa aktarma | `GET /export/xlsx` istek anında üretir (mevcut xlsx aracı; PDF adapter'la aynı desen). Tarihler Europe/Istanbul, tutarlar ₺ ondalık. İndirme `lib/api.ts` `downloadFile()` ile (KAS-06 ile aynı yardımcı). | HES-05 |
| İşletme durumu | `business.status` enum'una `deleting` eklenir: `pilot \| active \| payment_due \| suspended \| deleting`. `deleteRequestedAt` ve `deleteAt` (= +30 gün) alanları. Yazma istekleri Askıda ile aynı `403 TENANT_SUSPENDED` ile reddedilir; istemci `status` alanına göre şerit metnini seçer, ikinci bir kod yoktur. | HES-07 |
| Silme job'u | ADM-02'deki günlük job'a bir adım: `deleteAt <= şimdi` olan işletmeleri siler. K34'teki 12 ay sonrası silme ile **aynı fonksiyon**; işletmeye ait bütün tablolar ve depolama dosyaları silinir. E-postalar (talep, 7 gün kala, silindi) ADM-01 e-posta altyapısıyla. | HES-07 |
| Vazgeçme | `POST /business/delete/cancel`: `status` önceki değerine döner (`previousStatus` saklanır), `deleteAt` boşalır. | HES-07 |
| Şifre değiştirme | `POST /auth/change-password` (mevcut + yeni). Başarıda diğer cihazların refresh token'ları iptal edilir (KUR-03 ile aynı mekanizma). | HES-01 |
| Çıkış | İstemci token'ı siler ve TanStack Query önbelleğini temizler; sunucuda bu cihazın refresh token'ı iptal edilir. | HES-02 |
| İletişim bilgisi | Petzibu WhatsApp numarası ve e-postası uygulama yapılandırmasında sabittir (`lib/config.ts`); ADM-02 şeridi ve HES-06 aynı değeri kullanır. | HES-06 |
| Kod yeri | `features/settings/` (account, legal, export, danger). Ayarlar sekmesi `app/(tabs)/settings.tsx` yalnızca kompozisyon; işletme ayarları E1'in bileşenlerini bağlar. | Tümü |

## Diğer epic'lere bağlantılar

| Buradan | Oraya | Konu |
| --- | --- | --- |
| HES-01 | KUR-05…09, INT-01, HAT-01, HAT-02 | Ayarlar ekranı bu story'lerin giriş kapısıdır |
| HES-01 | KUR-03 | Şifre değişince oturum kapatma kuralı |
| HES-03 | KUR-01, INT-03, INT-05 | Onay modeli ortak |
| ADM-03 | INT-03 | Müşteri aydınlatma ve bakım riski metinleri |
| HES-04 | KAS-04, HAY-05 | Silme engelleri: borç ve ileri tarihli randevu |
| HES-04 | MUS-01, HAT-03, HAT-07 | Anonim müşteri listelerde görünmez |
| HES-04 | KAS-08 | Kasa geçmişi bozulmaz |
| HES-05 | KAS-06 | `downloadFile()` yardımcısı |
| HES-06 | ADM-02 | İptal sonrası Askıda |
| HES-07 | ADM-02 | Silinecek durumu ve silme job'u |

## Açık sorular

- Yok.
