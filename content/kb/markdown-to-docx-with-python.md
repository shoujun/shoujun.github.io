---
title: "How to convert Markdown to Word (.docx) without templates"
date: 2026-11-23
draft: false
tags: ["skills", "doc-conversion", "python", "tooling"]
description: "Convert markdown runbooks, notes and docs to native Word .docx using python-docx via the simple-md-docx-converter agent skill — headings, tables, task lists, code fences, mermaid diagrams and optional corporate templates."
---

## Problem

You need a `.docx` (Word) version of markdown content — runbooks, notes,
tracking docs — without hand-maintaining a template or a placeholder
contract. Pandoc output often needs post-editing; corporate recipients
just want a Word file.

## Solution

Use the released agent skill **`simple-md-docx-converter`**
(`~/skills/releases/simple-md-docx-converter`, also linked from
`~/skills/assets/skills/`). It converts any well-formed `.md` to native
Word constructs with `python-docx`:

- Headings, paragraphs, lists (incl. task lists), tables, code fences,
  blockquotes, strikethrough, horizontal rules
- `<mark>` highlighting, inline images, `{{toc}}` fields, page breaks,
  multi-file merge
- Mermaid diagrams render to images via `mmdc`/Kroki/`npx` when
  available, else degrade to labelled text
- Optional `--template` (.docx/.dotx/.dotm or a named template from
  `templates/`) inherits corporate styles/headers/footers and fills
  `{{key}}` tokens via `--set key=value`

## Quick start

```bash
# One-time: create the venv with python-docx
~/skills/releases/simple-md-docx-converter/scripts/setup.sh

# Convert (writes input.docx next to input.md)
~/skills/releases/simple-md-docx-converter/.venv/bin/python \
  ~/skills/releases/simple-md-docx-converter/scripts/convert_md_to_docx.py input.md

# Merge several files, branded output
convert_md_to_docx.py a.md b.md -o merged.docx \
  --template corporate-report --set title="Quarterly Report"
```

Useful flags: `--code-theme <pygments-name>`, `--code-font <name>`,
`--toc`, `--scan-placeholders`, `--open` (open on completion),
`--no-verify`.

## Gotchas

- Run `setup.sh` first — it bootstraps pip and creates `.venv`; use
  `--dev` to also get pytest/pygments, `--code-theme` for highlighting.
- The pipeline strips YAML frontmatter, so Hugo/Jekyll posts convert
  cleanly.
- TOC fields are marked `w:dirty` — Word prompts to update fields on
  open; that is expected.
- Verify output with `scripts/verify_docx_placeholders.py out.docx` —
  confirms no unresolved `{{tokens}}` and sane file size (≥ 20 KB).

## Related

- Skill source: `~/skills/releases/simple-md-docx-converter` (graduated,
  standalone repo); dev copy at `~/skills/assets/skills/simple-md-docx-converter`
- Stack membership: `doc-conversion.txt` in `~/skills/assets/stacks/`
