# Petzibu – Mevcut modüllerde rakiplerden öğrenilecekler

> Kaynak: Rakip analizi (23 Eylül 2026). 12 rakip: MoeGo, DaySmart Pet, Teddy, GrooMore, Gingr, PetExec, Pawfinity, Animalo, Salon Randevu, RandevuNet, RandevuLS, Kolay Randevu / SalonAppy.
> Kapsam: Yalnızca epics.md'de zaten olan E1–E8 modülleri. Yeni modül önerileri bu dosyada yok.

**Öncelik etiketleri**

- **MVP:** Az emekle mevcut epic'e eklenebilir.
- **Tasarım notu:** Şimdi kodlanmasa da veri modelinde veya UX'te şimdiden hesaba katılmalı.
- **Sonra:** Faz 2 veya sonrası.

---

## Özet: öne çıkan 6 öneri

1. **E6 · Hatırlatmaya tek tıkla onay linki.** Yarı otomatik modelin en büyük eksiğini, yani yanıtın okunamamasını, WhatsApp API'ye gerek kalmadan kapatır.
2. **E5 · Yapılandırılmış bakım raporu.** Davranış, cilt/kulak/tüy bulgusu ve sonraki bakım önerisi serbest metin yerine seçilebilir alanlar olur. Rakiplerin en güçlü etkileşim aracı bu.
3. **E2 · Referans fotoğrafı.** Profilde sabit duran "böyle kesilsin" fotoğrafı.
4. **E3 · Intake linki mevcut müşteriye de gönderilebilsin.** Aşı bilgisi kendiliğinden güncel kalır.
5. **E6 · Toplu hatırlatma kuyruğu.** Bildirimler tek tek gelmez; günün mesajları tek ekranda sırayla gönderilir.
6. **E8 · Veri erişim garantisi.** Abonelik bitse de veri salt okunur ve dışa aktarılabilir kalır. Güven vaadi olarak kullanılabilir.

---

## E1 · Kurulum & İşletme

| Rakip ne yapıyor                                                                                                                       | Petzibu için öneri                                                                                                                                                                      | Öncelik      |
| -------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| Gingr fiyatı ırk, kilo ve çoklu hayvan indirimi kurallarıyla belirliyor. Animalo'da hafta sonu fiyatı, Pawfinity'de saatlik fiyat var. | MVP'de yalnızca boy kademesi kalsın. Ama fiyat, hizmet tablosunda sabit bir sütun olarak değil, **kural** olarak modellensin. Irk, kilo ya da çoklu hayvan kuralı ileride eklenebilsin. | Tasarım notu |
| GrooMore ve PetExec hayvana özel fiyat tutuyor.                                                                                        | Hayvanın son ödediği fiyat hatırlansın ve bir sonraki randevuda önerilsin. Kademe fiyatı, "Paşa hep 500 ₺ öder" gibi durumları karşılamıyor.                                            | MVP          |
| MoeGo ve Teddy veri taşımayı ücretsiz yapıyor, Gingr $350, PetExec $300 alıyor.                                                        | Rakipten gelen müşteri için içe aktarma bir satış argümanı. Excel içe aktarma (Faz 2) başka yazılımların dışa aktarım formatlarını da kabul etsin.                                      | Sonra        |

## E2 · Müşteri & Hayvan

| Rakip ne yapıyor                                                                                                    | Petzibu için öneri                                                                                                                | Öncelik |
| ------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ------- |
| DaySmart'ta önce/sonra fotoğraflarından ayrı bir referans fotoğrafı var.                                            | Profilde sabitlenen "istenen kesim" fotoğrafı. Groomer değişse de sonuç tutarlı olur.                                             | MVP     |
| DaySmart aşı uyarısını randevuda olduğu kadar ödeme ekranında da gösteriyor. MoeGo takvimde renkli ikon kullanıyor. | Aşı durumu takvimde, randevu detayında ve tahsilat ekranında görünsün. Takvim kısmı zaten epic'te var.                            | MVP     |
| PetExec'te uyarı yalnızca hayvana değil, sahibine de konabiliyor ("geç kalır", "ödemede sorun").                    | Müşteri etiketleri takvim kartında da görünsün. Şu an yalnızca hayvan uyarı etiketleri takvimde.                                  | MVP     |
| GrooMore no-show geçmişini renkli etiketle gösteriyor.                                                              | Bizdeki müşteri özet metrikleri (gelmedi sayısı) bunu karşılıyor. Eşik aşılırsa (ör. 2 kez gelmedi) otomatik etiket önerilebilir. | Sonra   |

## E3 · Intake Form

| Rakip ne yapıyor                                                                                                                                                                            | Petzibu için öneri                                                                                                             | Öncelik |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ------- |
| Gingr PreCheck: randevudan önce mevcut müşteriye "bilgilerini güncelle, aşı belgesini yükle" linki gidiyor. Gingr bu adımda ek hizmet de satıyor ve işlem başına $15–25 artış iddia ediyor. | Intake linki mevcut müşteriye de gönderilebilsin. Telefonla eşleştirme zaten var. Aşı karnesi fotoğrafı alanı açıkça bulunsun. | MVP     |
| GrooMore'da sorular türe ve ırka göre değişiyor.                                                                                                                                            | Sabit şablonda en azından kedi ve köpek ayrımı olsun.                                                                          | MVP     |
| MoeGo, Gingr, GrooMore ve RandevuLS formda dijital onay alıyor.                                                                                                                             | Bakım riski onayı (keçe, kısa tıraş, yaşlı hayvan) onay kutusu olarak eklensin. Form builder gerekmez, K6 ile uyumlu.          | MVP     |

## E4 · Takvim & Randevu

| Rakip ne yapıyor                                          | Petzibu için öneri                                                                                                  | Öncelik |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------- |
| MoeGo Smart Schedule, boşluk bırakmayan saati öneriyor.   | Randevu oluştururken hizmet süresine sığan ilk boş saat önerilsin. Tasarımdaki "Müsait Saat" blokları bunun temeli. | MVP     |
| GrooMore'da ajanda (liste) görünümü var.                  | Mobilde saat ızgarasına ek olarak günün randevuları liste halinde görülebilsin. Liste daha hızlı taranıyor.         | Sonra   |
| 8 rakipte sürükle-bırak ile erteleme var.                 | Takvimde kartı sürükleyerek erteleme.                                                                               | MVP     |
| Gingr'da çoklu hayvanlı randevuda ek hayvan indirimi var. | E1'deki fiyat kuralına bağlı.                                                                                       | Sonra   |

## E5 · Randevu Operasyonu

| Rakip ne yapıyor                                                                                                            | Petzibu için öneri                                                                                                                                                                         | Öncelik                    |
| --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------- |
| Gingr (50 milyonun üzerinde rapor kartı gönderdiğini söylüyor) ve MoeGo'da rapor kartı yapılandırılmış alanlardan oluşuyor. | Rapora seçilebilir alanlar eklensin: davranış (sakin / huzursuz), cilt, kulak ve tüy bulguları, sonraki bakım önerisi. "Huzursuzdu" iki kez işaretlenirse hayvana uyarı etiketi önerilsin. | MVP                        |
| MoeGo, müşteri sonraki randevuyu almadıysa rapora randevu linki ekliyor.                                                    | Rapor mesajına rebook önerisi eklensin ("3 hafta sonra için yer ayıralım mı?"). Online randevu gelince bu bir linke dönüşür.                                                               | MVP (metin) / Sonra (link) |
| Gingr hayvan hazır olunca teslim alma bildirimi gönderiyor.                                                                 | "Paşa hazır, gelip alabilirsiniz" WhatsApp şablonu. Tek dokunuşla gönderilir.                                                                                                              | MVP                        |

## E6 · Hatırlatmalar

| Rakip ne yapıyor                                                                                                                   | Petzibu için öneri                                                                                                                                        | Öncelik                  |
| ---------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| DaySmart'ta müşteri SMS'e "C" yazarak onaylıyor. Salon Randevu onay linkini 79 ₺/ay eklenti olarak satıyor.                        | Hatırlatma mesajına tek tıkla onay linki eklensin. Müşteri tıklayınca randevu "Onaylandı" olur. Intake'teki hesapsız link altyapısı burada da kullanılır. | MVP                      |
| RandevuNet iki aşamalı hatırlatma gönderiyor (24 saat önce ve önceki akşam). Önce WhatsApp'tan gidiyor, iletilmezse SMS'e düşüyor. | Zamanlama ayarı birden fazla hatırlatmaya izin versin. SMS'e düşme Faz 2 konusu.                                                                          | MVP (ayar) / Sonra (SMS) |
| Teddy, GrooMore ve Animalo uzun süredir gelmeyen müşteriyi ayrıca yakalıyor.                                                       | "X haftadır gelmeyenler" listesi. Rebook tarihi kaydedilmemiş müşterileri de kapsar.                                                                      | MVP                      |
| Rakipler hatırlatmayı tamamen otomatik gönderiyor.                                                                                 | Yarı otomatik modelde günde 8–10 bildirim owner'ı yorabilir. Tek tek bildirim yerine "Bugün gönderilecek 8 mesaj" kuyruğu olsun, sırayla aç-gönder.       | MVP                      |

## E7 · Tahsilat & Kasa

| Rakip ne yapıyor                                                                                  | Petzibu için öneri                                                                                                                                          | Öncelik                    |
| ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| GrooMore kullanıcıları dökümün ancak ödeme alındıktan sonra gönderilebilmesinden şikâyetçi.       | Döküm tahsilattan önce de paylaşılabilsin.                                                                                                                  | MVP                        |
| Gingr içe aktarmada açılış borç bakiyelerini de taşıyor.                                          | Müşteri eklenirken ve Excel içe aktarmada açılış borcu girilebilsin. Deftere veresiye yazan kuaför için kritik.                                             | MVP (elle) / Sonra (Excel) |
| Pawfinity müşteri alacağını (store credit) değişiklik kaydıyla tutuyor.                           | Borç bakiyesi eksiye düşebilsin (fazla ödeme, ön ödeme). Faz 2'deki paket ve seans kartı bunun üzerine kurulur.                                             | Tasarım notu               |
| PetExec kullanıcılarının en büyük şikâyeti, komisyonun bakım gününe değil ödeme gününe yazılması. | Faz 2'deki prim hesaplamasının hizmet tarihine mi tahsilat tarihine mi göre yapılacağı şimdiden kararlaştırılsın. K2 gereği gelir tahsilat anında oluşuyor. | Tasarım notu               |
| DaySmart'ta gider takibi yalnızca en üst pakette var.                                             | Bizim avantajımız. Pazarlamada öne çıkarılsın.                                                                                                              | —                          |

## E8 · Hesap & Veri

| Rakip ne yapıyor                                                                                       | Petzibu için öneri                                                                         | Öncelik           |
| ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ | ----------------- |
| SalonAppy'nin Şikâyetvar'daki ana şikâyeti: hesap kapatılınca müşteri verisine erişilemiyor.           | Abonelik bitse de veri salt okunur kalsın ve dışa aktarılabilsin. Bunu açıkça vaat edelim. | MVP               |
| Salon Randevu'da ani zam şikâyetleri var (yıllık 2.800 ₺'den 12.000 ₺'ye).                             | Fiyat kilidi veya zam için önceden haber verme sözü.                                       | Sonra (iş modeli) |
| RandevuNet ve RandevuLS sitelerinde KVKK uyumunu öne çıkarıyor. Global rakiplerin hiçbirinde KVKK yok. | Bizde KVKK zaten MVP'de. Satış sayfasında görünür yapılsın.                                | —                 |

---

## Kaynaklar

- MoeGo: [grooming](https://www.moego.pet/pet-grooming-software), [rapor kartı](https://help.moego.pet/en/articles/11370580-custom-report-card-transform-your-pet-care-experience-with-personalized-pet-reports), [aşı](https://www.moego.pet/help/en/articles/12845387-pet-vaccine)
- DaySmart Pet: [aşı uyarıları](https://help.daysmartpet.com/en/articles/9301492), [hatırlatmalar](https://help.daysmartpet.com/en/articles/12451730), [fiyat](https://www.daysmart.com/pet/pricing/)
- Gingr: [PreCheck](https://www.gingrapp.com/precheck), [rapor kartı](https://www.gingrapp.com/blog/engage-customers-with-gingrs-report-cards), [fiyat kuralları](https://support.gingrapp.com/hc/en-us/articles/29735161124493), [veri aktarımı](https://support.gingrapp.com/hc/en-us/articles/28089331857549)
- PetExec: [uyarılar](https://www.petexec.net/features/pet-and-owner-advisories), [Capterra yorumları](https://www.capterra.com/p/92864/PetExec/reviews/)
- GrooMore: [özellikler](https://www.groomore.com/all-features.html), [Play Store](https://play.google.com/store/apps/details?id=com.groomore.scheduler&hl=en_US)
- Teddy: [hatırlatmalar](https://www.tryteddy.com/texting-reminders)
- Pawfinity: [müşteri alacağı](https://www.pawfinity.com/features/client-account-credit-store-credit-gift-cards/)
- Animalo: [grooming](https://animalo.com/en/for_grooming)
- RandevuNet: [ana sayfa](https://randevunet.com/)
- Salon Randevu: [paketler](https://www.salonrandevu.app/paket-liste.php), [Şikâyetvar](https://www.sikayetvar.com/salonrandevucom)
- SalonAppy: [Şikâyetvar](https://www.sikayetvar.com/salonappy)
