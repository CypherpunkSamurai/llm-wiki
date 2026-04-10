---
name: llm-wiki
description: "Build and maintain a persistent personal knowledge base. Ingests sources (URLs, files, pasted text), processes them into topic pages, and answers questions with citations. Trigger when the user mentions 'add to wiki', 'knowledge base', 'ingest', or 'research this'."
license: MIT
metadata:
    skill-author: CypherpunkSamurai
---


# LLM Wiki

Persistent, append-only knowledge base. Raw sources → processed references → topic knowledge.

<routing>
## Determine Pipeline

Identify what the user wants and read the corresponding pipeline file (relative to this SKILL.md):

| Intent | Pipeline |
|--------|----------|
| Add a source (URL, file, paste) | [[wiki-add.md]](./pipelines/wiki-add.md) |
| Research a topic exhaustively, follow tangents | [[wiki-research.md]](./pipelines/wiki-research.md) |
| Ask a question about existing knowledge | [[wiki-query.md]](./pipelines/wiki-query.md) |
| Correct or append to existing articles | [[wiki-update.md]](./pipelines/wiki-update.md) |
| Check wiki health, fix broken links | [[wiki-check.md]](./pipelines/wiki-check.md) |

Read only the pipeline you need. Do not load all of them.
</routing>

<structure>
## Wiki Structure

All paths relative to project root.

```
wiki/
├── index.md            # Main index (paginate into index-1.md at ~200 entries)
├── log.md              # Append-only action log
├── raw/                # Immutable source material, never modify after creation
│   └── <topic>/        # Topic dir, mirrors topics/ structure
│       ├── articles/
│       ├── papers/
│       ├── books/
│       ├── conversations/
│       └── other/
├── topics/             # Processed knowledge, append and correct only, never delete
│   └── <topic>/
│       └── <article>.md
└── archives/           # Frozen query snapshots, never updated
```

- `raw/<topic>/<type>/` — Immutable. Topic-first organization, then file type. Text gets metadata header. Binaries as-is.
- `topics/` — Processed knowledge. Max 2 levels of subdirs. Append-only growth.
- `archives/` — Frozen, point-in-time snapshots of synthesized answers. Never cascade-updated, never merged to topic articles. Only cite `topics/` articles, not `raw/` sources.
- `index.md` — One row per article, grouped by topic, link plus summary.
- `log.md` — Every action logged.
- Dates in content: log entries get `[YYYY-MM-DD HH:MM]`, raw metadata headers get Published date, corrections get date annotation. No dates in filenames. Filesystem modification time for article "last updated".
</structure>

<init>
## Initialization

On first use, create missing structure. Never overwrite existing files.

Required: `wiki/`, `wiki/raw/`, `wiki/topics/`, `wiki/archives/`, `wiki/index.md` (heading only), `wiki/log.md` (heading only).

If Query or Check can't find wiki structure: tell user to add something first.
</init>

<conventions>
## Conventions

- Standard markdown, relative links throughout.
- `topics/` max 2 levels. `raw/<topic>/<type>/` mirrors topics/ structure.
- Inside wiki files: relative links. In conversation: project-root-relative paths.
- Knowledge only grows — append and correct, never delete or reduce topic content.
- Binaries in `raw/` referenced by `.md` counterpart in `topics/`.
- All operations update `log.md`. Add/Research/Archive/Update also update `index.md`.
- Templates live in [./references/](./references/) relative to this file. Read when you need exact format.
</conventions>
