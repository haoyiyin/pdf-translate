# PDF Translation with Layout Preservation

Translate PDF documents between languages while **fully preserving original layout, images, tables, formulas, and typography**.

Uses [PDFMathTranslate](https://github.com/PDFMathTranslate/PDFMathTranslate) (`pdf2zh`) — the open-source tool accepted at **EMNLP 2025 System Demonstrations** — with Google Translate as the free default backend. No API key needed.

## Features

- ✅ **Layout preservation** — images, tables, formulas, fonts, typography all kept intact
- ✅ **Multi-language** — support for 20+ languages (Chinese, English, Japanese, Korean, French, German, etc.)
- ✅ **Dual output** — generates both single-language (`*-mono.pdf`) and bilingual side-by-side (`*-dual.pdf`) versions
- ✅ **Free backend** — Google Translate, no API key required
- ✅ **Multiple backends** — also supports DeepL, OpenAI, Ollama, Claude, and more
- ✅ **Batch processing** — translate entire folders at once
- ✅ **GUI mode** — browser interface at localhost:7860
- ✅ **Online PDFs** — translate documents directly from URLs

## Quick Start

```bash
# Install
pip install pdf2zh

# Translate a Chinese PDF to English
pdf2zh document.pdf -li zh -lo en -s google

# Output: document-mono.pdf (English) + document-dual.pdf (bilingual)
```

## Usage Examples

### Translate between any languages
```bash
# Chinese → English
pdf2zh input.pdf -li zh -lo en -s google

# English → Chinese
pdf2zh input.pdf -li en -lo zh -s google

# Japanese → English
pdf2zh input.pdf -li ja -lo en -s google

# French → Chinese
pdf2zh input.pdf -li fr -lo zh -s google
```

### Common language codes

| Language | Code |
|----------|------|
| Chinese (Simplified) | `zh` |
| Chinese (Traditional) | `zh-TW` |
| English | `en` |
| Japanese | `ja` |
| Korean | `ko` |
| French | `fr` |
| German | `de` |
| Spanish | `es` |
| Russian | `ru` |
| Arabic | `ar` |
| Portuguese | `pt` |
| Italian | `it` |

### Specify output directory
```bash
pdf2zh input.pdf -li zh -lo en -s google -o ./output/
```

### Translate specific pages
```bash
pdf2zh input.pdf -li zh -lo en -s google -p 1-10
```

### From URL
```bash
pdf2zh https://example.com/document.pdf -li zh -lo en -s google
```

### Batch translate folder
```bash
pdf2zh --dir ./pdfs/ -li zh -lo en -s google
```

### GUI mode
```bash
pdf2zh -i
# Opens browser at http://localhost:7860
```

### Use different backends
```bash
# DeepL (higher quality, needs API key)
pdf2zh input.pdf -li zh -lo en -s deepl

# OpenAI (needs API key)
pdf2zh input.pdf -li zh -lo en -s openai

# Local LLM via Ollama (free)
pdf2zh input.pdf -li zh -lo en -s ollama
```

## Options Reference

| Flag | Description | Example |
|------|-------------|---------|
| `-li` | Source language | `-li zh` |
| `-lo` | Target language | `-lo en` |
| `-s` | Translation service | `-s google` |
| `-p` | Specific pages | `-p 1,3,5-10` |
| `-o` | Output directory | `-o ./output/` |
| `-t` | Thread count | `-t 4` |
| `-f` | Exclude text (regex) | `-f "(MS.*)"` |
| `--dir` | Batch translate folder | `--dir ./pdfs/` |
| `-i` | Launch GUI | `-i` |
| `--compatible` | Compatibility mode | `--compatible` |
| `--ignore-cache` | Bypass translation cache | `--ignore-cache` |

## Hermes Agent Skill

This repo includes a [Hermes Agent](https://hermes-agent.nousresearch.com) skill (`SKILL.md`). If you use Hermes Agent, the skill is automatically available when you mention translating a PDF.

## Troubleshooting

### Model download failure
pdf2zh downloads a layout detection model on first run. If blocked:
```bash
export HF_ENDPOINT=https://hf-mirror.com
```

### Rate limits
For large documents, split the pages:
```bash
pdf2zh document.pdf -li zh -lo en -s google -p 1-20
pdf2zh document.pdf -li zh -lo en -s google -p 21-40
```

### Rendering issues
Try compatibility mode:
```bash
pdf2zh document.pdf -li zh -lo en -s google --compatible
```

## License

MIT
