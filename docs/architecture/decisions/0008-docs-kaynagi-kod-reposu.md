# ADR-0008: Dokümanların kaynağı kod reposudur; site onu build sırasında kopyalar

Durum: Kabul edildi · Tarih: 2026-09-25

## Bağlam

Dokümanlar iki yerde yaşıyordu: `petzibu-docs` (MkDocs sitesi, epic'ler ve ürün kararları K1…K58) ve kod reposu (`RULES.md`, mimari doküman). İkisi elle senkron tutuluyordu ve sapma zaten gerçekleşti: aynı epic dosyaları eski `petzibu` reposunda ve docs reposunda birbirinden farklı sürümlerle duruyordu, `rules.md` ise kopyaydı.

Sapmanın nedeni yapısaldır: bir epic kararı koda dokunan bir PR'da değişir, ama dokümanı başka bir repoda ayrıca güncellemek gerekir. Aynı anda iki repoda yazılması gereken her şey, bir süre sonra yalnızca birinde yazılır.

## Karar

- Bütün dokümanların **tek kaynağı** kod reposudur: `docs/` (epic'ler, mimari, ADR'ler, kaynaklar) ve uygulama kökündeki `RULES.md` dosyaları. Bir kararın değiştiği PR dokümanı da taşır.
- `petzibu-docs` yalnızca **yayın kabuğudur**: `mkdocs.yml`, tema, `requirements.txt`, giriş sayfası ve deploy workflow'u. İçerik dosyası tutmaz.
- Kopyalama yönü **çekmedir**: `petzibu-docs`'un deploy workflow'u build sırasında kod reposunu checkout eder, `docs/` ağacını ve `RULES.md` dosyalarını `docs/` altına kopyalar, sonra `mkdocs build` çalıştırır. Kod reposu docs reposuna yazmaz.
- Kod reposu özel olduğu için workflow'a yalnızca okuma yetkisi olan bir token (`PETZIBU_REPO_TOKEN`) verilir.

## Sonuçlar

- Dokümanı güncellemek için ikinci bir repoya PR açılmaz; sapma imkânı ortadan kalkar.
- Site içeriği üretilmiş dosyadır: `petzibu-docs` içine elle doküman eklenmez, eklenirse ilk build'de kaybolur.
- `mkdocs.yml`'deki `nav` ağacı kod reposundaki dosya adlarına bağlıdır. Doküman taşınır veya adı değişirse `nav` da aynı işte güncellenir; `mkdocs build --strict` kırık bağlantıyı build'de yakalar.
- Kod reposundaki bir doküman herkese açık olmayabilir. Site şifre korumalıdır (`encryptcontent`), ama yayına girmemesi gereken bir dosya `nav` dışında bırakılır.
- Mobil ve backend kuralları siteye `rules-mobile.md` ve `rules-backend.md` olarak çıkar; kaynakları `apps/mobile/RULES.md` ve `apps/api/RULES.md`'dir.
- Bazı göreli yollar iki ağaçta aynı olamaz: `docs/architecture/backend-architecture.md` depoda `../rules-backend.md`'ye, sitede `../rules-backend.md`'ye bakar. Kopyalama adımı bu dönüşümü yapar ve listeyi kısa tutar; kaynak depoda doğru kalır. Aynı şekilde `decisions/index.md` sitede `decisions/index.md` olur (MkDocs bölüm girişini böyle bekler).
- Kopyalama bugün `scripts/sync-docs.sh` ile **elle** çalıştırılır. Çekme workflow'u kurulana kadar site, script en son ne zaman çalıştıysa o kadar günceldir.
