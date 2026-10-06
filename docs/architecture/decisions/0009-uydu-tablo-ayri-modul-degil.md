# ADR-0009: Uydu tablo ayrı modül değildir, aynı modülde ikinci servistir

Durum: Kabul edildi · Tarih: 2026-10-01

## Bağlam

E5 hazırlığında bakım raporu (`GroomingReport`, OPR-04) kendi domain modülü olarak planlandı ve mimari dokümana `appointments ← grooming-reports` bağımlılık satırı olarak yazıldı. Rapor ayrı bir modül olunca `appointments`'ın üstüne çıkıyor; ama randevu detayı hayvan başına son raporu, tamamlama da rebook önerisini (K49) taşımak zorunda. Yani `appointments` raporu okumak istiyor, rapor da randevuyu — ters yön doğuyor ve `forwardRef` yasak (§4.2). Çözüm olarak dördüncü bir orkestrasyon modülü (`appointment-overview`) planlandı ve `/appointments` uçlarının tamamı oraya taşınacaktı.

Bu, mevcut üç orkestrasyon modülüyle aynı desen gibi görünüyor ama bir yerde ayrışıyor. `customer-overview`, `pet-overview` ve `closed-day-overview` **var olan** bir ters yönü çözüyor: hayvan detayı randevuya bakıyor, randevu da hayvana (`pets ← appointments ← pets`). Döngü, iki tablonun birbirine gerçekten ihtiyaç duymasından geliyor ve kaçınılmaz. Raporda böyle bir zorunluluk yok: döngü, raporu ayrı modüle koyma kararının **kendi ürettiği** bir döngüdür. Katman da onu çözmek için açılıyor.

Repoda bu şeklin zaten bir cevabı var. `PetPhoto` hayvana 1:1 bağlı, kendi yükleme ve referans ömrü olan bir tablodur; ayrı modül olmadı, `PetsModule` içinde `PetPhotosService` oldu ve modül yorumundaki gerekçe şudur: "ikisini ayrı modüllere bölmek `/pets` yolunu iki yere dağıtırdı." `ClosedDaysService` aynı sebeple `businesses` içindedir. `GroomingReport` da `AppointmentPet`'e 1:1 bağlıdır, onsuz var olamaz ve bugün `appointments` dışında onu okuyan hiçbir modül yoktur: "Huzursuz" etiket önerisi istemcide türetilir (HAY-02), E6 ve E7 rapora bakmaz.

Servis dosyasının şişmesi ayrı modül gerekçesi değil: `appointments.service.ts` 1148 satır ve büyümemeli, ama modül başına birden çok servis zaten kuraldır (`pets` iki, `businesses` üç).

## Karar

- **Bir tabloyu ayrı modüle çıkarmanın ölçütü, sahibi dışında bir tüketicisi olmasıdır** — kendi enum'ları, kendi prisma dosyası ya da kendi uçları olması değil. Tek tüketicisi ebeveyn aggregate olan 1:1 / 1:N uydu tablo, ebeveynin modülünde kalır ve kendi **servis dosyasını** alır.
- `GroomingReport` `appointments` modülünün tablosudur. Rapor mantığı `modules/appointments/grooming-reports.service.ts`'te yaşar; `grooming-reports` modülü açılmaz.
- Rapor uçları `/appointments/:appointmentId/pets/:petId/report` yolunda ve `appointments.controller.ts` içindedir. `/appointments` uçlarının tamamı tek controller'da kalır.
- `appointment-overview` **açılmaz**. `AppointmentsModule` controller'ını korur.
- Prisma dosyası modül sınırını belirlemez: `grooming-reports.prisma` ayrı dosya olarak kalır, `catalog.prisma`'nın iki modülün tablosunu tutması gibi.

## Sonuçlar

- Mimari dokümandaki (§4.2) `appointments ← grooming-reports` bağımlılık satırı ve `appointment-overview` paragrafı kalkar. Orkestrasyon katmanı yalnızca **kaçınılmaz** ters yönler için açılır; kendi ürettiğimiz bir döngüyü çözmek için açılmaz.
- `appointments` modülü üç servisli olur: randevu, durum geçişi, bakım raporu. Üçü de export edilir (E6'nın onay sayfası durum servisini çağıracak).
- Tenant kuralları değişmez: `GroomingReport` `TENANT_MODELS`'te kalır ve cross-tenant e2e testi yine zorunludur (§4.2, RULES.md). Tablo sahipliği modül düzeyindedir, dosya düzeyinde değil.
- Rapora bir gün `appointments` dışından bir tüketici çıkarsa (ör. hayvan detayında rapor geçmişi) karar yeniden bakılır. Mantık zaten ayrı bir servis dosyasında olduğu için taşıma mekaniktir; o gün ters yön **gerçek** olur ve orkestrasyon katmanı hak edilir.
