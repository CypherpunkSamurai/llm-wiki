---
name: llm-wiki
description: "Build and maintain a persistent personal knowledge base. Ingests sources (URLs, files, pasted text), processes them into topic pages, and answers questions with citations. Trigger when the user mentions 'add to wiki', 'knowledge base', 'ingest', or 'research this'."
license: MIT
metadata:
    skill-author: CypherpunkSamurai
---


# LLM Wiki

Save-only knowledge base. Raw sources → processed references → topic pages.

<schema>
## Pipeline Schema

Pipeline files use simple blocks for small models:

- `<info>`: what this does and why
- `<critical>`: rules you must follow
- `<tool-choices>`: tools to try, in order
- `<step>`: steps to follow in order

Keep blocks short and clear. Put goals in `info`, rules in `critical`, tool order in `tool-choices`, and steps in `step`.
</schema>

<routing>
## Pick the Right Pipeline

Find what the user wants and read only that pipeline file (paths from this SKILL.md):

| What user wants | Pipeline |
|--------|----------|
| Add a source (URL, file, pasted text) | [[wiki-add.md]](./pipelines/wiki-add.md) |
| Research a topic in depth, follow side topics | [[wiki-research.md]](./pipelines/wiki-research.md) |
| Ask a question from existing knowledge | [[wiki-query.md]](./pipelines/wiki-query.md) |
| Fix or add to existing pages | [[wiki-update.md]](./pipelines/wiki-update.md) |
| Check wiki health, fix broken links | [[wiki-check.md]](./pipelines/wiki-check.md) |

Read only the pipeline you need. Do not read all of them.
After reading a pipeline, follow its `<critical>` rules before any `<step>`.
</routing>

<structure>
## Wiki Structure

All paths are relative to project root.

```
wiki/
├── index.md            # Main index (split into index-1.md at ~200 entries)
├── log.md              # Save-only action log
├── raw/                # Source material, never change after saving
│   └── <topic>/        # Topic folder, matches topics/ structure
│       ├── articles/
│       ├── papers/
│       ├── books/
│       ├── conversations/
│       └── other/
├── topics/             # Processed knowledge, add and fix only, never delete
│   └── <topic>/
│       └── <article>.md
└── archives/           # Saved query answers, never changed
```

- `raw/<topic>/<type>/` — Never change. Topic first, then file type. Text gets a metadata header. Binaries stay as-is.
- `topics/` — Processed knowledge. Max 2 folder levels. Add-only growth.
- `archives/` — Saved query answers from a point in time. Never update, never merge into topic pages. Only cite `topics/` pages, not `raw/` sources.
- `index.md` — One line per page, grouped by topic, link and short summary.
- `log.md` — Log every action.
- Dates in content: log entries use `[YYYY-MM-DD HH:MM]`, raw metadata headers use Published date, fixes get date notes. No dates in filenames. Use file modification time for article "last updated".
</structure>

<init>
## First-Time Setup

On first use, create missing folders and files. Never overwrite existing files.

Needed: `wiki/`, `wiki/raw/`, `wiki/topics/`, `wiki/archives/`, `wiki/index.md` (heading only), `wiki/log.md` (heading only).

If Query or Check can't find wiki structure: tell user to add something first.
</init>

<conventions>
## Rules

- Standard markdown, use relative `[[cross]]` links.
- `topics/` max 2 folder levels. `raw/<topic>/<type>/` matches topics/ structure.
- Inside wiki files: use relative links. In chat: use project-root-relative paths.
- Default to add-only updates. Fix old claims only when the knowledge has changed, been replaced, or is clearly wrong.
- Knowledge only grows — add and fix when needed, never delete or shrink topic content.
- Binaries in `raw/` get linked by `.md` page in `topics/`.
- All actions update `log.md`. Add/Research/Archive/Update also update `index.md`.
- Templates are in [./references/](./references/) relative to this file. Read them when you need exact format.
</conventions>
