# petzibu-docs

Petzibu ürün ve teknik dokümantasyonunun yayın kabuğu. [MkDocs](https://www.mkdocs.org/) + [Material](https://squidfunk.github.io/mkdocs-material/) ile derlenir, GitHub Pages'e çıkar.

## İçerik buraya elle eklenmez

Dokümanların tek kaynağı kod reposudur (`oguzhanklss/petzibu`): `docs/` ağacı ve uygulama kökündeki `RULES.md` dosyaları (ADR-0008). Bu repo `mkdocs.yml`'i, temayı, `docs/index.md`'yi ve deploy workflow'unu tutar; geri kalan her şey kopyadır.

`docs/` altında **üretilen** yollar:

```
docs/epics/            # kod reposu: docs/epics/
docs/architecture/     # kod reposu: docs/architecture/ (ADR'ler dahil)
docs/resources/        # kod reposu: docs/resources/
docs/rules-backend.md  # kod reposu: apps/api/RULES.md
docs/rules-mobile.md   # kod reposu: apps/mobile/RULES.md
```

Bunları düzenleme; bir sonraki kopyalamada kaybolur. Düzeltme kod reposunda yapılır.

## Senkronizasyon

Bugün elle, kod reposundaki script ile:

```bash
cd ../petzibu && ./scripts/sync-docs.sh   # varsayılan hedef ../petzibu-docs
cd ../petzibu-docs && mkdocs build --strict
```

Script kopyaladığı ağaçları önce siler, `decisions/README.md`'yi `index.md` yapar ve depoda doğru olup sitede kırılan göreli yolları (`../../apps/api/RULES.md` gibi) site yollarına çevirir.

Yeni bir doküman eklendiğinde `mkdocs.yml`'deki `nav` da güncellenir; `mkdocs build --strict` hem kırık bağlantıyı hem `nav` dışında kalan sayfayı yakalar.

Otomatik çekme (docs reposunun workflow'unun kod reposunu checkout etmesi) henüz kurulmadı — ADR-0008'in hedefi budur.

## Kurulum

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Geliştirme

```bash
mkdocs serve        # http://127.0.0.1:8000
mkdocs build --strict
```

Site şifre korumalıdır (`encryptcontent`); şifre `DOCUMENTATION_PASSWORD` secret'ından gelir.
