# ADR-0004: Tenant izolasyonu Prisma extension ve CLS ile zorlanır

Durum: Kabul edildi · Tarih: 2026-09-24

## Bağlam

Tenant işletmedir (K5); işletmeye ait her tabloda `businessId` bulunur. Boilerplate'te tenant kavramı yoktu; onun yerine `deletedAt: null` filtresini otomatik ekleyen bir soft-delete extension'ı vardı. İzolasyonu her sorguda elle `where: { businessId }` yazarak sağlamak, unutulan tek bir sorguda cross-tenant sızıntı demektir. Postgres RLS alternatifi değerlendirildi: Prisma ile bağlantı başına `SET` gerektirir, connection pool ile karışır ve tek client kuralını bozar.

## Karar

- `nestjs-cls` istek bağlamını taşır. Auth guard token'daki `businessId`'yi CLS'e yazar; istemci `businessId` göndermez.
- `database/tenant.extension.ts` tenant tablolarındaki her sorguya `businessId` filtresini ekler, `create`'lerde alanı doldurur. Tenant tablolarının listesi extension'da tek yerdedir.
- Context'siz bir tenant sorgusu **hata fırlatır**. Varsayılan davranış yoktur.
- Tenant'sız çalışan kod (admin iç aracı, zamanlanmış işler) context'i `runAsSystem()` ile, bir işletme adına çalışan job `runAsTenant(businessId)` ile açıkça kurar. Public web işletmeyi linkteki token'dan bulur ve `runAsTenant` kullanır.
- Transaction'lar `@nestjs-cls/transactional` ile aynı CLS üzerinden yürür; adapter'a verilen client tenant extension'lı client'tır. Uygulamada tek Prisma client vardır.
- Raw SQL tenant tablolarında yasaktır; zorunlu yerlerde `businessId` parametresi alan fonksiyonlar tek dosyada toplanır.

## Sonuçlar

- Soft-delete extension'ı ve `deletedAt` alanları kalkar; silme kuralları tür bazında ayrı tanımlıdır (mimari doküman §5.7).
- Her modül için cross-tenant e2e testi zorunludur: A'nın token'ıyla B'nin kaydına erişim `404` döner. Extension'ı doğrulayan şey bu testlerdir.
- Extension tenant tablosu listesine eklenmeyen yeni bir tablo izole edilmez; yeni Prisma modeli ekleyen PR listeyi de günceller. Bunu yakalamak için extension listesi ile şemadaki `businessId` içeren modeller tip seviyesinde karşılaştırılır.
