# PDF Chinese → English Translation (pdf2zh)

Translate Chinese PDF documents to English while **fully preserving original layout, images, tables, formulas, and typography**.

Uses [PDFMathTranslate](https://github.com/PDFMathTranslate/PDFMathTranslate) (`pdf2zh`) — the open-source tool accepted at **EMNLP 2025 System Demonstrations** — with Google Translate as the free backend. No API key needed.

## Features

- ✅ **Layout preservation** — images, tables, formulas, fonts, typography all kept intact
- ✅ **Dual output** — generates both English-only (`*-mono.pdf`) and bilingual side-by-side (`*-dual.pdf`) versions
- ✅ **Free backend** — Google Translate, no API key required
- ✅ **Multiple backends** — also supports DeepL, OpenAI, Ollama, and 20+ translation services
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

### Basic translation
```bash
pdf2zh input.pdf -li zh -lo en -s google
```

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
pdf2zh https://example.com/chinese-document.pdf -li zh -lo en -s google
```

### Batch translate folder
```bash
pdf2zh --dir ./chinese_pdfs/ -li zh -lo en -s google
```

### GUI mode
```bash
pdf2zh -i
# Opens browser at http://localhost:7860
```

## Options Reference

| Flag | Description | Example |
|------|-------------|---------|
| `-li` | Source language | `-li zh` (Chinese) |
| `-lo` | Target language | `-lo en` (English) |
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

This repo includes a [Hermes Agent](https://hermes-agent.nousresearch.com) skill (`SKILL.md`). If you use Hermes Agent, the skill is automatically available — just mention "translate this Chinese PDF" and it will use the workflow above.

## Troubleshooting

### Model download failure
pdf2zh downloads a layout detection model on first run. If blocked:
```bash
export HF_ENDPOINT=https://hf-mirror.com
```

### Rate limits
For large documents with Google Translate, split the pages:
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
