---
name: gb-official-doc-enhanced
description: Use when formatting, proofreading, drafting, auditing, or exporting Chinese official documents, formal notices, reports, requests, letters, minutes, school administrative materials, .docx Word files, GB/T 9704-2012 layouts, GB/T 15834-2011 punctuation, red-header metadata, imprint/page-number checks, or local HTML one-click official-document workflows.
---

# 国标公文增强校排

## Overview

This is a trial enhanced skill for Chinese official-document work. It combines local GB/T official-document proofreading scripts with KaguraNanaga document-format CLI presets, while keeping formal-document elements conservative by default.

## Mode Decision

| User request | Mode | Main entry |
| --- | --- | --- |
| 普通通知、报告、方案、申报材料、公文格式排版 | `body` | `scripts/one_click_enhanced.py` |
| 只查问题，不生成正式版 | `audit` | `scripts/one_click_enhanced.py --mode audit` |
| 红头文件、发文字号、版记、页码、抄送等完整要素 | `full` | `scripts/one_click_enhanced.py --mode full` with metadata |
| 信函、命令（令）、纪要特殊格式 | `letter` / `command` / `minutes` | `scripts/one_click_enhanced.py --mode ...` |
| 学术、法律、自定义模板或 Kagura/docformat-gui 预设 | preset workflow | `scripts/dfskills/process.py` |
| 从 Markdown/txt 生成 Word | text workflow | `scripts/dfskills/from_text.py` |
| 本地网页上传下载 | local HTML | `scripts/web_app.py` or platform launcher |

Default to `body` when the user does not explicitly provide official metadata. Do not invent 发文机关、发文字号、签发人、印章、抄送机关、印发机关、印发日期, or版记.

## Official One-Click Workflow

Use the bundled Codex Python when available:

```bash
PY=/Users/zhangjun/.cache/codex-runtimes/codex-primary-runtime/dependencies/python/bin/python3
$PY scripts/one_click_enhanced.py input.docx --mode body --json
```

For full official format, require metadata that is actually known:

```bash
$PY scripts/one_click_enhanced.py input.docx --mode full \
  --issue-org "某某机关" \
  --doc-number "某发〔2026〕1号" \
  --imprint-org "某某机关办公室" \
  --imprint-date "2026年7月5日" \
  --page-numbers \
  --json
```

Return the generated `.docx` path and Markdown report path. State the mode used, standards applied, automatic fixes, unresolved metadata gaps, and remaining judgment-based issues.

## Kagura Preset Workflow

Use this when the user asks for broader Chinese document cleanup, academic/legal/custom preset behavior, or wants to reference KaguraNanaga `document-format-skills` / `docformat-gui` behavior:

```bash
PY=/Users/zhangjun/.cache/codex-runtimes/codex-primary-runtime/dependencies/python/bin/python3
$PY scripts/dfskills/process.py smart input.docx output.docx --preset official
$PY scripts/dfskills/process.py analyze input.docx --json
$PY scripts/dfskills/from_text.py input.md output.docx --title "工作方案"
```

Use `assets/presets/templates.json` and `assets/presets/custom_settings.json` when a docformat-gui-compatible preset is needed.

## References To Load

- For rule conflicts or default choices, read `references/conflict-resolution.md`.
- For GB/T 9704-2012 layout details, read `references/gbt9704-format.md`.
- For GB/T 15834-2011 punctuation details, read `references/gbt15834-punctuation.md`.
- For Word/OpenXML implementation details, read `references/docx-implementation.md`.
- For final QA, read `references/audit-checklist.md`.
- For drafting/templates, read `references/document-templates.md` and `references/writing-techniques.md`.
- For upstream provenance, read `references/upstream-integration.md`.

## Non-Negotiable Rules

- Preserve meaning and paragraph order.
- Use GB/T 9704-2012 plus GB/T 15834-2011 as the default official-document basis.
- Treat `body` mode as ordinary formal-material formatting: no invented red header, document number, seal, page number, or版记.
- Treat `full` mode as metadata-driven: create/repair full official elements only when the source contains them or the user supplies them.
- Use Arabic dates like `2026年7月5日`; use `〔〕` in document numbers; do not add `第` or leading zeroes to 发文字号 sequence numbers.
- Normalize high-confidence punctuation and typo issues; report uncertain semantic choices instead of silently rewriting them.
- Ensure Chinese quotes `“”` render with Chinese font slots in Word, not Times New Roman.
- Keep original files unchanged; write new outputs.

## Local HTML App

For a small desktop-like workflow:

```bash
PY=/Users/zhangjun/.cache/codex-runtimes/codex-primary-runtime/dependencies/python/bin/python3
$PY scripts/web_app.py --no-open
```

On macOS, users can double-click `scripts/run_official_doc_mac.command`. On Windows, use `scripts/run_official_doc_windows.bat`.
