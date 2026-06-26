---
name: pdf-translate
description: "Use when translating PDF documents between languages while preserving original layout, images, tables, and formulas using pdf2zh (PDFMathTranslate)."
version: 1.0.0
author: Yi
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [PDF, Translation, Documents, Multi-language, Google-Translate]
    related_skills: [ocr-and-documents]
---

# PDF Translation with Layout Preservation (pdf2zh)

Translate PDF documents between languages while **fully preserving original layout, images, tables, formulas, and typography**. Uses `pdf2zh` (PDFMathTranslate, accepted at EMNLP 2025) with Google Translate as the free default backend.

Output: two files — `*-mono.pdf` (translated only) and `*-dual.pdf` (bilingual side-by-side).

## When to Use

- You need to translate a PDF from one language to another
- The original layout, images, and formatting must be preserved
- You want a bilingual version for comparison
- The document contains tables, formulas, or complex layouts
- Scanned PDFs or documents with embedded images

**Don't use for:** simple text extraction without layout preservation (use `ocr-and-documents` skill instead).

## Prerequisites

```bash
# Install pdf2zh
pip install pdf2zh
```

No API key needed for Google Translate backend (free, no sign-up).

## Usage

### Basic Translation

```bash
# Translate Chinese to English
pdf2zh input.pdf -li zh -lo en -s google

# Translate English to Chinese
pdf2zh input.pdf -li en -lo zh -s google

# Translate Japanese to English
pdf2zh input.pdf -li ja -lo en -s google

# Translate French to Chinese
pdf2zh input.pdf -li fr -lo zh -s google
```

This generates:
- `input-mono.pdf` — translated-only version
- `input-dual.pdf` — bilingual version (original left, translated right)

### Specify Custom Output Directory

```bash
pdf2zh input.pdf -li zh -lo en -s google -o ./output/
```

### Translate Specific Pages Only

```bash
pdf2zh input.pdf -li zh -lo en -s google -p 1-10
```

### Translate from URL (Online PDF)

```bash
pdf2zh https://example.com/document.pdf -li zh -lo en -s google
```

### Batch Translate an Entire Folder

```bash
pdf2zh --dir ./pdfs/ -li zh -lo en -s google
```

### GUI Mode (Browser Interface)

```bash
pdf2zh -i
# Opens at http://localhost:7860
```

### Use Different Translation Backends

| Backend | Command | API Key Needed |
|---------|---------|----------------|
| Google (free) | `-s google` | No |
| DeepL | `-s deepl` | Yes (higher quality) |
| OpenAI | `-s openai` | Yes |
| Ollama (local) | `-s ollama` | No (local LLM) |
| Azure | `-s azure` | Yes |
| Claude | `-s claude` | Yes |

### Language Codes (Common)

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

## Parameter Reference

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

## One-Shot Recipes

### Quick Translate a Local PDF

```bash
pip install -q pdf2zh && pdf2zh input.pdf -li zh -lo en -s google
```

### Translate with Higher Quality (OpenAI)

```bash
pdf2zh input.pdf -li zh -lo en -s openai
# Requires OPENAI_API_KEY in environment
```

## Common Pitfalls

1. **Model download failure**: pdf2zh downloads a layout detection model (`wybxc/DocLayout-YOLO-DocStructBench-onnx`) on first run. If blocked by network, set:
   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```

2. **Large PDF processing**: For documents with 100+ pages, increase thread count:
   ```bash
   pdf2zh input.pdf -li zh -lo en -s google -t 4
   ```

3. **Translation rate limits**: Very large documents may hit the translation service's rate limit. Try splitting the document:
   ```bash
   pdf2zh input.pdf -li zh -lo en -s google -p 1-20
   pdf2zh input.pdf -li zh -lo en -s google -p 21-40
   ```

4. **Compatibility issues**: If the output has rendering problems, try compatibility mode:
   ```bash
   pdf2zh input.pdf -li zh -lo en -s google --compatible
   ```

## Verification Checklist

- [ ] `*-mono.pdf` exists and contains translated text
- [ ] `*-dual.pdf` exists with bilingual layout
- [ ] Images/graphics from original are preserved
- [ ] Tables and formulas render correctly
- [ ] Page count matches original
