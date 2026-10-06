# ADR-0010: Döküm ara toplamı `billing`'de saklanır; döküm uçları orkestrasyondadır

Durum: Kabul edildi · Tarih: 2026-10-06

## Bağlam

E7 hazırlığında (BS5) iki yerleşik karar birbirine çarptı.

Mimari doküman bağımlılık yönünü `appointments → billing` olarak kuruyor (§4.2): randevu tamamlanınca `appointments.complete()` aynı transaction'da `billing.createStatement()` çağırır (§5.3). Aynı doküman `customer-overview`'un bakiyeyi ve `privacy`'nin silme engelindeki borcu doğrudan `billing`'den okuyacağını söylüyor; yani `billing`'in bir müşterinin borcunu **tek başına** cevaplaması bekleniyor.

E7 teknik notu ise randevu dökümünün satırlarını kopyalamıyor: "Randevu dökümünün satırları ayrı bir kopya değildir; tamamlanmış randevunun `appointmentLine` kayıtlarıdır… Böylece satırların tek bir kaynağı olur." OPR-03 bu kararın üstüne kuruldu (ek ücret satırının `extraChargeId`'si KAS-02 için nullable kaldı, `PUT /appointments/:id` ek ücretlere dokunmuyor) ve KAS-02 ile eklenen satırın `durationMin` taşıyıp randevunun `endAt`'ını değiştirmemesi bu modele göre yazıldı.

İkisi birlikte tutmuyor. Dökümün ara toplamı satırların toplamıdır; satırlar `appointments`'ın tablosundadır; `billing` başka modülün tablosunu Prisma ile okuyamaz (§4.2 madde 1) ve `AppointmentsService`'i de çağıramaz, çünkü yön `appointments → billing` ve döngü yasak. Dolayısıyla `billing` ne kalanı ne borcu kendi başına hesaplayabiliyor. Üç çıkış yolu değerlendirildi:

1. **Satırları `StatementLine`'a kopyalamak.** `billing` kendine yeter, orkestrasyon gerekmez. Ama E7 notunu ve OPR-03'ün "tek kaynak" kararını tersine çevirir: KAS-02 düzenlemesinden sonra randevu detayındaki `total` ile döküm toplamı ayrışır, müşteri geçmişindeki randevu satırı eski tutarı gösterir.
2. **Toplamları çağıranın taşıması.** Hiçbir şey saklanmaz; `billing` açık dökümleri (indirim + ödeme) verir, ara toplamları her çağıran (`customer-overview`, `privacy`, `appointments`, döküm uçları) `appointments`'tan alıp `billing`'in saf fonksiyonuna geçirir. İlkelere en sadık yol, ama "Borçlu" süzgeci ve alacaklar listesi bütün tamamlanmış randevuların satırlarını okumak zorunda kalır ve borç kuralı dört yerde kurulur.
3. **`Statement.subtotal` saklamak.** Satırlar `appointmentLine`'da kalır; ara toplam `Statement` üzerinde durur ve satırları değiştiren her yol onu aynı transaction'da yazar. `billing` borcu, kalanı ve borçlu listesini kendi tablolarından tek sorguyla verir.

Üçüncü yol seçildi. Bu, "hesaplanan değer saklanmaz" alışkanlığından bir sapmadır (döküm durumu, kalan, borç ve alacak saklanmaz ve öyle kalır); gerekçesi **tek yazıcı** olmasıdır. Tamamlanmış randevunun satırını değiştiren yol bugün yok (`PUT /appointments/:id`, ek ücret ekleme ve kaldırma `409 APPOINTMENT_FINALIZED` veriyor) ve E7 tek bir yol açıyor: döküm düzenlemesi (KAS-02). O yol satırı ve `subtotal`'ı aynı transaction'da yazdığı sürece ikisi ayrışamaz. Repoda emsali var: `Appointment.endAt` satır sürelerinin toplamından türer ve saklanır, çünkü onu yazan tek yol satırları yazan yoldur.

Satır **içeriği** ise yine `appointments`'tadır ve döküm gövdesi onu göstermek zorundadır. `billing` okuyamaz, `appointments` döküm bilmez; birleştiren katman orkestrasyondur ve §4.2'nin "kaçınılmaz ters yön" ölçütünü karşılar — iki tablo birbirine gerçekten ihtiyaç duyuyor, döngü bir tasarım kararının ürettiği bir şey değil (ADR-0009'un karşı örneği).

## Karar

- **`Statement.subtotal` saklanır.** Değeri her zaman dökümün satırlarının toplamıdır. Yazan iki yol vardır ve ikisi de satırları yazan transaction'ın içindedir: tamamlama (`appointments.complete` → `billing.createForAppointment(customerId, appointmentId, subtotal)`) ve döküm düzenlemesi (`statements` orkestrasyonu → `appointments.rewriteCompletedLines` → `billing.setSubtotal`). Açılış dökümünde ara toplam tek `StatementLine`'ın tutarıdır ve aynı yerde yazılır.
- **`billing` para kurallarının tek sahibidir ve hiçbir domain modülüne bağımlı değildir.** Çekirdekten yalnızca `businesses`'a bakar (saat dilimi; `appointments` ile aynı), katman yönü `çekirdek ← domain` bunu zaten izin veriyor. Ara toplamı çağırandan alır; indirim (K43, yarım yukarı yuvarlama), kalan, döküm durumu, müşteri borcu, borçlu listesi ve K41 dağıtımı yalnızca `Statement`, `StatementLine` ve `Payment` tablolarından hesaplanır. `balancesOf`, `debtorIds`, `receivables`, `hasBalance` tek çağrıdır; çağıranlar `appointments`'a gitmez.
- **`/statements` uçları `statements` orkestrasyon modülündedir.** Döküm gövdesi `billing` (para), `appointments` (satır içeriği) ve `customers` (ad) verisini birleştirir; PDF `businesses` (salon bilgisi) ve `document-generator`'ı ekler. `billing`'in controller'ı yoktur; `customers` ve `pets` ile aynı durumdadır.
- **`AppointmentsService.rewriteCompletedLines` tek istisnadır.** Tamamlanmış randevunun satırlarını değiştiren tek yoldur, yalnızca `completed` randevuyu kabul eder, hayvan listesine dokunmaz, `endAt`'ı yeniden hesaplamaz ve yalnızca `statements` orkestrasyonundan çağrılır. `PUT /appointments/:id` ve ek ücret uçları `409 APPOINTMENT_FINALIZED` vermeye devam eder.
- **Bağımlılık yönü değişmez:** `appointments → billing`. Ters yönde çağrı yoktur.

## Sonuçlar

- `billing.prisma`'ya `subtotal Int` eklenir (migration, BS5-01). Prisma yorumu bu ADR'ye atıf verir.
- Satır yazan her e2e `subtotal == Σ satır` eşitliğini de iddia eder; KAS-02 düzenlemesinin ödenenin altına düşen toplamı `409` ile reddetmesi satır yazımını da geri alır (tek transaction).
- Mimari doküman §4.1 ağacına `statements/` orkestrasyon satırı, §4.2 orkestrasyon listesine `statements`, §5.5'e `subtotal` istisnası girer. `customer-overview` ve `privacy`'nin borcu `billing`'den tek çağrıyla okuduğu cümleler doğru kalır.
- "Hesaplanan değer saklanmaz" ilkesi genel kural olarak sürer; `subtotal` bu ADR'yle sınırlı, gerekçeli bir istisnadır. Tamamlanmış randevunun satırını değiştiren **ikinci bir yol** açılırsa (ör. web back office'te doğrudan satır düzenleme) o yol da `setSubtotal`'ı aynı transaction'da çağırmak zorundadır; aksi hâlde bu karar yeniden bakılır.
- E7 teknik notlarının "Döküm modeli", "Hesaplanan değerler" ve "Modül sınırı" satırları bu ADR'ye göre güncellendi.
