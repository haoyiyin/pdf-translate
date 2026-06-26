---
name: pdf-zh-en-translate
description: "Translate Chinese PDFs to English while preserving layout, images, tables, and formulas using pdf2zh (PDFMathTranslate) with Google Translate."
version: 1.0.0
author: Yi
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [PDF, Translation, Chinese, English, Google-Translate, Documents]
    related_skills: [ocr-and-documents]
---

# PDF Chinese → English Translation (pdf2zh)

Translate Chinese PDF documents to English while **fully preserving original layout, images, tables, formulas, and typography**. Uses `pdf2zh` (PDFMathTranslate, accepted at EMNLP 2025) with Google Translate as the free backend.

Output: two files — `*-mono.pdf` (English only) and `*-dual.pdf` (bilingual side-by-side).

## When to Use

- You have a Chinese PDF that needs to be translated to English
- The layout, images, and formatting must be preserved
- You need a bilingual version for comparison
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

### Basic — Translate Chinese PDF to English

```bash
pdf2zh input.pdf -li zh -lo en -s google
```

This generates:
- `input-mono.pdf` — English-only version
- `input-dual.pdf` — bilingual version (Chinese left, English right)

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
pdf2zh https://example.com/chinese-document.pdf -li zh -lo en -s google
```

### Batch Translate an Entire Folder

```bash
pdf2zh --dir ./chinese_pdfs/ -li zh -lo en -s google
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
| DeepL | `-s deepl` | Yes |
| OpenAI | `-s openai` | Yes |
| Ollama (local) | `-s ollama` | No |

## Parameter Reference

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

## One-Shot Recipes

### Quick Translate a Local PDF

```bash
pip install -q pdf2zh && pdf2zh document.pdf -li zh -lo en -s google
```

### Translate with Higher Quality (OpenAI)

```bash
pdf2zh document.pdf -li zh -lo en -s openai
# Requires OPENAI_API_KEY in environment
```

## Common Pitfalls

1. **Model download failure**: pdf2zh downloads a layout detection model (`wybxc/DocLayout-YOLO-DocStructBench-onnx`) on first run. If blocked by network, set:
   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```

2. **Large PDF processing**: For documents with 100+ pages, increase thread count:
   ```bash
   pdf2zh document.pdf -li zh -lo en -s google -t 4
   ```

3. **Google Translate rate limits**: Very large documents may hit Google's rate limit. Try splitting the document:
   ```bash
   pdf2zh document.pdf -li zh -lo en -s google -p 1-20
   pdf2zh document.pdf -li zh -lo en -s google -p 21-40
   ```

4. **Compatibility issues**: If the output has rendering problems, try compatibility mode:
   ```bash
   pdf2zh document.pdf -li zh -lo en -s google --compatible
   ```

## Verification Checklist

- [ ] `*-mono.pdf` exists and contains English text
- [ ] `*-dual.pdf` exists with bilingual layout
- [ ] Images/graphics from original are preserved
- [ ] Tables and formulas render correctly
- [ ] Page count matches original
