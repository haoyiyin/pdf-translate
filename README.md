# PDF Translation Agent Skill

An agent skill for translating PDF documents between 20+ languages while **fully preserving original layout, images, tables, formulas, and typography**.

**Agent-compatible** — Works with any AI agent that supports the [Agent Skills specification](https://agentskills.io/specification): Claude Code, OpenClaw, Hermes, Codex, Cursor, Windsurf, and more.

Uses [PDFMathTranslate](https://github.com/PDFMathTranslate/PDFMathTranslate) (`pdf2zh`) — the open-source tool accepted at **EMNLP 2025 System Demonstrations** — with Google Translate as the free default backend. No API key needed.

## How Agents Use This Skill

When an agent loads this skill, it can translate PDFs automatically by running `pdf2zh`:

```bash
# Agent installs and runs this automatically
pip install -q pdf2zh && pdf2zh document.pdf -li zh -lo en -s google

# Output: document-mono.pdf (translated) + document-dual.pdf (bilingual)
```

The agent handles language detection, backend selection, error recovery (model downloads, rate limits), and output verification.

## Installation

### 🤖 Via AI Agent (Recommended)

Tell any AI coding agent:

> "Install the PDF translation skill from `haoyiyin/pdf-translate`"

Your agent will clone, install dependencies, and configure the skill automatically. Works with Pi, Claude Code, Cursor, Codex, Copilot, and any agent supporting the [Agent Skills specification](https://agentskills.io/specification).

### 🛠️ Manual Installation

```bash
pip install pdf2zh
```

## Features

- ✅ **Layout preservation** — images, tables, formulas, fonts, typography all kept intact
- ✅ **Multi-language** — support for 20+ languages (Chinese, English, Japanese, Korean, French, German, etc.)
- ✅ **Dual output** — generates both single-language (`*-mono.pdf`) and bilingual side-by-side (`*-dual.pdf`) versions
- ✅ **Free backend** — Google Translate, no API key required
- ✅ **Multiple backends** — also supports DeepL, OpenAI, Ollama, Claude, and more
- ✅ **Batch processing** — translate entire folders at once
- ✅ **GUI mode** — browser interface at localhost:7860
- ✅ **Online PDFs** — translate documents directly from URLs

## Usage

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

## Agent Skill

This repository is an [Agent Skill](https://agentskills.io/specification). See [Installation](#installation) above for setup. Once installed, any compliant agent (Pi, Claude Code, Cursor, Codex, Copilot, etc.) can translate PDFs automatically via the included `SKILL.md`.

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
