# petzibu-docs

Petzibu ürün dokümantasyonu. [MkDocs](https://www.mkdocs.org/) + [Material](https://squidfunk.github.io/mkdocs-material/) ile derlenir.

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

## Yapı

```
docs/
  index.md          # ana sayfa
  epics/            # epic haritası (index.md) + E1–E8
  resources/        # araştırma, rakip analizi
  rules.md          # petzibu/RULES.md kopyası
mkdocs.yml
```
