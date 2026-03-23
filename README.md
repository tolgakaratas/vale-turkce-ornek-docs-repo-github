# Türkçe Yazım Denetimi — Örnek Proje

[![Türkçe Yazım Denetimi](https://github.com/Denomas/Turkce-yazim-denetimi-ornek/actions/workflows/vale.yml/badge.svg)](https://github.com/Denomas/Turkce-yazim-denetimi-ornek/actions/workflows/vale.yml)

Bu proje, [Türkçe Yazım Denetimi](https://github.com/Denomas/Turkce-yazim-denetimi) aracının nasıl kullanıldığını canlı olarak gösterir. Aşağıdaki entegrasyon yöntemlerini doğrudan inceleyebilir ve kendi projenize uyarlayabilirsiniz.

---

## Proje Yapısı

```
├── .pre-commit-config.yaml          ← Pre-commit entegrasyonu
├── .github/workflows/vale.yml       ← GitHub Actions entegrasyonu
└── docs/
    ├── tr/                           ← Türkçe belgeler (Vale denetimi burada çalışır)
    │   ├── giris.md                      Hatasız belge örneği
    │   └── hatali-belge.md               Bilerek hatalı belge (Vale hataları yakalar)
    └── en/                           ← İngilizce belgeler (Vale denetimi çalışmaz)
        └── readme.md
```

---

## Nasıl Çalışır?

### 1. Pre-commit ile (Her Commit'te Otomatik Denetim)

`.pre-commit-config.yaml` dosyanıza ekleyin:

```yaml
repos:
  - repo: https://github.com/Denomas/Turkce-yazim-denetimi
    rev: v1
    hooks:
      - id: Turkce-yazim-denetimi
        args: [--minAlertLevel=warning]
        files: ^docs/tr/     # Sadece Türkçe belgeleri denetle
```

Kurulum:
```bash
pip install pre-commit
pre-commit install
```

Artık her commit'te `docs/tr/` altındaki dosyalar otomatik olarak denetlenir.

### 2. GitHub Actions ile (CI/CD'de Otomatik Denetim)

`.github/workflows/vale.yml` dosyanıza ekleyin:

```yaml
name: Türkçe Yazım Denetimi

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  turkce-denetim:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - uses: Denomas/Turkce-yazim-denetimi@v1
        with:
          files: 'docs/tr/'
          min_alert_level: 'warning'
          no_spelling: 'true'         # Hunspell yazım denetimini kapat (isteğe bağlı)
```

### 3. GitLab CI ile

GitLab CI entegrasyonu için [GitLab örnek projesine](https://code.denomas.com/denomas/Turkce-yazim-denetimi-ornek) bakın.

---

## Bu Projede Neler Test Ediliyor?

| Test | Dosya | Beklenen Sonuç |
|------|-------|----------------|
| Temiz belge | `docs/tr/giris.md` | Hata yok, CI yeşil |
| Hatalı belge | `docs/tr/hatali-belge.md` | Vale hataları yakalar (bitişik yazım, Plaza Türkçesi, yanlış Türkçe) |
| İngilizce belge | `docs/en/readme.md` | Vale çalışmaz (sadece `docs/tr/` denetlenir) |

### Yakalanan Hata Örnekleri

`docs/tr/hatali-belge.md` dosyasında bilerek şu hatalar bırakılmıştır:

- **Bitişik yazım:** `herşey` → `her şey`, `birşey` → `bir şey`
- **Plaza Türkçesi:** `meeting`, `ASAP`, `deadline`, `feedback`
- **Yanlış Türkçe:** `yanlız` → `yalnız`, `herkez` → `herkes`, `süpriz` → `sürpriz`
- **Uzun cümle:** 30+ kelimelik cümle uyarısı

---

## Kendi Projenize Ekleme

1. Yukarıdaki pre-commit veya GitHub Actions yapılandırmasını kopyalayın
2. `files` parametresini kendi Türkçe belgelerinizin bulunduğu klasöre ayarlayın
3. Detaylı rehber: [Türkçe Yazım Denetimi Dokümantasyonu](https://github.com/Denomas/Turkce-yazim-denetimi)

---

*Bu proje [Denomas](https://denomas.com) tarafından geliştirilmektedir.*
