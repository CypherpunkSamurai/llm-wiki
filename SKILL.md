---
name: knowledge
description: "Build and maintain a persistent personal knowledge base. Use when: ingesting sources (URLs, files, pasted text), querying existing knowledge, researching topics, processing raw material into topic pages, or any mention of 'add to wiki', 'what do I know about', 'knowledge base', 'ingest', 'research this'. Trigger aggressively — if the user is accumulating, organizing, or retrieving structured knowledge, this skill applies."
---

# Knowledge

Persistent, append-only knowledge base. Raw sources → processed references → topic knowledge.

<structure>
```
wiki/
├── index.md            # Main index (paginate into index-1.md, index-2.md at ~200 entries)
├── log.md              # Append-only action log
├── raw/                # Immutable source material — read-only after creation
│   ├── articles/
│   ├── books/
│   ├── papers/
│   └── conversations/
├── topics/             # Processed reliable knowledge — append & correct, never replace
│   ├── attention-mechanisms/
│   │   └── flash-attn.md
│   └── hypervisors/
│       └── vmware.md
└── archives/           # Point-in-time query snapshots — never updated
    └── transformer-comparison.md
```

- `raw/` — Immutable. Organized by type subdirs. Optional topic nesting within type. Text gets metadata header. Binaries stored as-is.
- `topics/` — Processed knowledge. Up to 2 levels of subdirs. Each `.md` synthesizes from raw sources. Knowledge only grows — append and correct, never delete or replace content.
- `archives/` — Frozen query snapshots. Never cascade-updated.
- `index.md` — One row per article, grouped by topic, with link + summary. Dates come from filesystem.
- `log.md` — Every action gets an entry.
</structure>

<init>
On first use of any operation, create missing structure. Never overwrite existing files.

Required: `wiki/`, `wiki/raw/`, `wiki/topics/`, `wiki/archives/`, `wiki/index.md` (heading only), `wiki/log.md` (heading only).

If Query or Lint can't find wiki structure: tell user to run an ingest first.
</init>

---

## Operations

<research>
### Research

User wants to explore a topic or add to existing knowledge.

**Flow:**
1. Fuzzy grep `wiki/index*.md` for the topic
2. **Topic exists** → read existing article(s), identify gaps, research to fill gaps, append new info to existing article. Never replace existing content — only append or correct with attribution.
3. **Topic doesn't exist** → research the topic, create raw reference(s) first, then process into new topic article.

In both cases, always create raw references before modifying topics.
</research>

<ingest>
### Ingest

User provides a source (URL, file, paste). Two phases, always both.

#### Phase 1: Fetch → raw/

1. Acquire content via available tools. If unreachable, ask user to paste.
2. Classify source type → pick `raw/` subdir (`articles/`, `books/`, `papers/`, `conversations/`, `other/`). Create if missing. Optional topic nesting.
3. Save as `raw/<type>/<optional-topic>/descriptive-slug.md`
   - Slug from title, kebab-case, max 60 chars
   - Duplicate names → append numeric suffix
   - Metadata header per `references/raw-template.md`
   - Preserve original text. Clean formatting noise only.
   - Binaries: save directly, no metadata header needed.

#### Phase 2: Process → topics/

Determine placement:
- **Same thesis as existing article** → Append to that article. Add new source to metadata. Update affected sections.
- **New concept** → Create new article. Name after the concept, not the raw file.
- **Processed binary** → Create `<original-filename>.md` linking back to raw.
- **Spans topics** → Place in most relevant dir. Add See Also cross-references.

Not mutually exclusive. One source may merge into one article and spawn another.

**Conflicts:** If new source contradicts existing content, annotate disagreement with source attribution inline. Cross-link conflicting articles.

Article format: `references/topic-template.md`

#### Phase 3: Cascade

After primary article:
1. Scan same topic dir for affected articles
2. Scan index entries in other topics for related concepts
3. Update materially affected articles

Archives are never cascade-updated.

#### Phase 4: Post-Ingest

1. Update `wiki/index.md` — add/update entries for every touched article. Format per `references/index-template.md`. Paginate at ~200 entries.
2. Append to `wiki/log.md`:
   ```
   ## ingest | <primary article title>
   - Source: <raw file path>
   - Updated: <cascade-updated article title>
   ```
</ingest>

<query>
### Query

Search wiki and answer from accumulated knowledge.

1. Read `wiki/index*.md` to locate relevant articles
2. Read articles, synthesize answer
3. Prefer wiki content over training knowledge. Cite with: `[Title](wiki/topics/<topic>/<article>.md)`
4. Output in conversation. No file writes unless asked.

**Archive:** When user explicitly asks to save the answer:
1. Write to `wiki/archives/` per `references/archive-template.md`. Always new page, never merge.
2. Update index (prefix summary with `[Archived]`)
3. Log: `## query | Archived: <page title>`
</query>

<correct>
### Correct

Append to or correct existing knowledge without re-ingesting.

1. Locate target article(s) in `wiki/topics/`
2. Apply correction with strikethrough + attribution:
   ```markdown
   ~~Previous claim~~ **Corrected:** New info. (Source: <attribution>)
   ```
   Or append new information to appropriate section.
3. Run cascade update logic
4. Update index if summary changed
5. Log: `## correct | <article title> — <brief reason>`
</correct>

<lint>
### Lint

Quality checks. Two categories.

**Auto-fix:**
- Index consistency — missing files ↔ missing entries
- Broken internal links — search for moved files, fix if unique match
- Broken raw references — same approach
- See Also — add obvious missing cross-refs, remove dead links

**Report only:**
- Factual contradictions across articles
- Outdated claims superseded by newer sources
- Orphan pages with no inbound links
- Raw files never processed into a topic article

Log: `## lint | <N> issues found, <M> auto-fixed`
</lint>

---

<conventions>
## Conventions

- Standard markdown, relative links throughout
- `topics/` max 2 levels of subdirs. `raw/` any reasonable depth.
- Dates: use filesystem modification time. No manual timestamps in content except in raw metadata headers and correction annotations.
- Inside wiki files: relative links. In conversation: project-root-relative paths.
- Knowledge only grows. Append and correct, never delete or reduce topic content.
- Binary files in `raw/` are referenced by their `.md` counterpart in `topics/`.
- All operations update `log.md`. Ingest/Archive/Correct also update `index.md`.
</conventions>

<templates>
## Templates

All in `references/` relative to this file. Read when you need exact format.

| Template | For |
|----------|-----|
| `references/raw-template.md` | Raw source metadata header |
| `references/topic-template.md` | Topic article format |
| `references/archive-template.md` | Archived query result |
| `references/index-template.md` | Index page format |
| `references/processed-binary-template.md` | Processed binary files |
</templates>
