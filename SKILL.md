---
name: knowledge
description: "Build, maintain, and query a persistent personal knowledge base. Use when: ingesting raw sources (URLs, PDFs, files, pasted text) into the wiki, querying or searching existing knowledge, processing raw material into reliable topic pages, linting wiki health, or any mention of 'add to wiki', 'what do I know about', 'knowledge base', 'ingest this', 'process this source', or 'wiki'. Trigger aggressively — if the user is accumulating, organizing, or retrieving structured knowledge, this skill applies."
---

<!--
  KNOWLEDGE SKILL
  ===============
  A persistent, compounding knowledge base managed by LLMs.

  Core philosophy (from Karpathy):
    "The LLM writes and maintains the wiki; the human reads and asks questions."
    "The wiki is a persistent, compounding artifact."

  Pipeline:
    raw data --> process to reference [append & correct over time] --> knowledge [append & correct over time]

  The human curates sources, directs analysis, and asks questions.
  The LLM does the summarizing, cross-referencing, filing, and bookkeeping.
-->

# Knowledge

A skill for building and maintaining a persistent, compounding personal knowledge base. You manage a wiki with three internal layers: `raw/` (immutable source material), `topics/` (fully processed reliable knowledge), and root-level index/log files that tie everything together.

Knowledge compounds over time. Each new source enriches existing articles, adds cross-references, and flags conflicts. Nothing is re-derived on every query — the synthesis is always up to date.

---

## Architecture

<architecture>
All paths below are relative to `wiki/` which lives at the project root.

```
project_root/
└── wiki/
    ├── index.md              # Main knowledge index (paginated if large)
    ├── index-1.md            # Overflow page when index.md exceeds ~200 entries
    ├── index-n.md            # Further pagination as needed
    ├── log.md                # Append-only changelog of all operations
    ├── raw/                  # Immutable source material — read, never modify
    │   ├── articles/
    │   │   └── 2026-04-11-attention-is-all-you-need.md
    │   ├── books/
    │   │   └── linux/
    │   │       └── linux-kernel.pdf
    │   ├── papers/
    │   │   └── flash-attention-v2.pdf
    │   └── conversations/
    │       └── 2026-04-11-debugging-session.md
    ├── topics/               # Fully processed, reliable knowledge
    │   ├── attention-mechanisms/
    │   │   └── flash-attn.md
    │   ├── books/
    │   │   └── linux/
    │   │       └── linux-kernel.pdf.md   # Processed PDF → links back to raw
    │   └── hypervisors/
    │       └── vmware.md
    └── archives/             # Point-in-time query snapshots
        └── transformer-comparison.md
```

### Layer Responsibilities

**`wiki/raw/`** — Immutable source material. You read from here, never modify. Organized by source-type subdirectories (`articles/`, `books/`, `papers/`, `conversations/`, etc.), each with optional topic nesting. Binary files (PDFs, images) are stored as-is. Text sources get a metadata header.

**`wiki/topics/`** — Fully processed, reliable knowledge. You have full ownership. Organized by topic subdirectories with optional one-level nesting for sub-topics. Each `.md` file is a self-contained knowledge document that synthesizes information from one or more raw sources. For processed binary files (e.g., PDFs), use `<original-filename>.md` as the filename and link back to the raw source.

**`wiki/archives/`** — Point-in-time snapshots from query operations. These are synthesized answers the user chose to persist. They are never cascade-updated.

**`wiki/index.md`** — Global index. One row per topic article, grouped by topic directory, with link + summary + last-updated date. Paginate into `index-1.md`, `index-2.md`, etc. when the main index exceeds ~200 entries.

**`wiki/log.md`** — Append-only operation log. Every ingest, query-archive, lint, and correction gets a timestamped entry.
</architecture>

---

## Initialization

<initialization>
Triggers on first use of any operation. Check whether `wiki/` and its subdirectories exist. Create only what is missing — never overwrite existing files.

Required structure:
- `wiki/` directory
- `wiki/raw/` directory
- `wiki/topics/` directory
- `wiki/archives/` directory
- `wiki/index.md` — heading `# Knowledge Base Index`, empty body
- `wiki/log.md` — heading `# Wiki Log`, empty body

If a Query or Lint operation cannot find the wiki structure, tell the user:
"No wiki found. Run an ingest first to initialize the knowledge base."
Do not auto-create directories during Query or Lint — only during Ingest.
</initialization>

---

## Operations

There are four core operations. Each one follows strict rules to keep the knowledge base healthy and compounding.

---

### 1. Ingest

<ingest>
Fetch a source into `raw/`, then process it into `topics/`. Always both steps, no exceptions.

#### Phase 1: Fetch (raw/)

1. **Acquire the source content** using whatever tools your environment provides (file read, web fetch, URL download, user paste). If nothing can reach the source, ask the user to paste it directly.

2. **Classify the source type** and pick the appropriate `raw/` subdirectory:
   - `articles/` — web articles, blog posts, documentation pages
   - `books/` — book chapters, full books, textbook excerpts
   - `papers/` — academic papers, research reports, whitepapers
   - `conversations/` — chat logs, meeting transcripts, interview notes
   - `other/` — anything that doesn't fit the above
   
   Create the subdirectory if it doesn't exist. Add optional topic nesting within the type directory when it makes sense (e.g., `raw/books/linux/`).

3. **Save the source.**

   **For text sources:** save as `raw/<type>/<optional-topic>/YYYY-MM-DD-descriptive-slug.md`
   - Slug from source title, kebab-case, max 60 characters
   - Published date unknown → omit the date prefix, set Published metadata to `Unknown`
   - Duplicate filenames → append numeric suffix (e.g., `-2.md`)
   - Include metadata header per `references/raw-template.md`
   - Preserve original text faithfully. Clean formatting noise. Do not rewrite opinions.

   **For binary sources (PDFs, images, etc.):** save the binary file directly into the appropriate `raw/` subdirectory. No metadata header needed — the file itself is the source.

#### Phase 2: Process (topics/)

This is where raw material becomes reliable knowledge. You are synthesizing, not copying.

**Determine where the new content belongs:**

- **Same core thesis as existing article** → Merge into that article. Add the new source to the Sources/Raw metadata. Update affected sections. Increment the Updated date.
- **New concept** → Create a new article in the most relevant topic directory. Name the file after the concept, not the raw file.
- **Processed binary file** → Create `<original-filename>.md` in the relevant topic directory (e.g., `topics/books/linux/linux-kernel.pdf.md`). This markdown document contains the extracted and synthesized content, with a link back to the raw binary.
- **Spans multiple topics** → Place in the most relevant directory. Add See Also cross-references to related articles elsewhere.

These are not mutually exclusive. A single source may warrant merging into one article while also creating a new article for a distinct concept it introduces.

**Conflict handling:** If the new source contradicts existing content, annotate the disagreement with source attribution. When merging, note the conflict inline. When the conflicting content lives in separate articles, note it in both and cross-link them.

See `references/topic-template.md` for the article format.

#### Phase 3: Cascade Updates

After the primary article, check for ripple effects:

1. Scan articles in the same topic directory for content affected by the new source.
2. Scan `wiki/index.md` entries in other topics for articles covering related concepts.
3. Update every article whose content is materially affected. Refresh its Updated date.

Archive pages are never cascade-updated — they are point-in-time snapshots.

#### Phase 4: Post-Ingest

1. **Update `wiki/index.md`**: Add or update entries for every touched article. When adding a new topic section, include a one-line description. The Updated date reflects when the article's knowledge content last changed. See `references/index-template.md` for format. If index.md exceeds ~200 entries, paginate into `index-1.md`, etc.

2. **Append to `wiki/log.md`**:
   ```
   ## [YYYY-MM-DD] ingest | <primary article title>
   - Source: <raw file path>
   - Updated: <cascade-updated article title>
   - Updated: <another cascade-updated article title>
   ```
   Omit `- Updated:` lines when no cascade updates occur.
</ingest>

---

### 2. Query

<query>
Search the wiki and answer questions using accumulated knowledge.

**Trigger examples:**
- "What do I know about X?"
- "Summarize everything related to Y"
- "Compare A and B based on my wiki"
- "What sources do I have on Z?"

#### Steps

1. Read `wiki/index.md` (and pagination pages if they exist) to locate relevant articles.
2. Read those articles and synthesize an answer.
3. **Prefer wiki content over your own training knowledge.** The wiki is the user's curated source of truth. Cite with markdown links: `[Article Title](wiki/topics/<topic>/<article>.md)` (project-root-relative paths for in-conversation citations).
4. Output the answer in the conversation. Do not write files unless asked.

#### Archiving Query Results

When the user explicitly asks to archive or save the answer to the wiki:

1. Write the answer as a new page in `wiki/archives/`. See `references/archive-template.md`.
   - Sources: markdown links to the wiki articles cited in the answer
   - No Raw field — content does not come from raw/
   - File name reflects the query topic (e.g., `transformer-architectures-overview.md`)
2. Always create a new page. Never merge into existing articles — archive content is a synthesized answer, not raw material.
3. Update `wiki/index.md`. Prefix the Summary with `[Archived]`.
4. Append to `wiki/log.md`:
   ```
   ## [YYYY-MM-DD] query | Archived: <page title>
   ```
</query>

---

### 3. Correct

<correct>
Append to or correct existing knowledge. This is how the wiki evolves without re-ingesting.

**Trigger examples:**
- "Actually, flash attention v2 also supports..." 
- "Update the vmware article to mention..."
- "The claim about X is wrong, it should be Y"

#### Steps

1. Locate the target article(s) in `wiki/topics/`.
2. Apply the correction or addition. If correcting, use strikethrough for the old claim and note the correction source:
   ```markdown
   ~~Previous claim~~ **Corrected [YYYY-MM-DD]:** New accurate information. (Source: <attribution>)
   ```
   If appending new information, add it to the appropriate section.
3. Update the article's Updated date.
4. Run cascade update logic — other articles referencing the corrected claim may need updating.
5. Update `wiki/index.md` if the summary changed.
6. Append to `wiki/log.md`:
   ```
   ## [YYYY-MM-DD] correct | <article title>
   - Reason: <brief description of what changed>
   ```
</correct>

---

### 4. Lint

<lint>
Quality checks on the wiki. Two categories with different authority levels.

#### Deterministic Checks (auto-fix)

Fix these automatically:

**Index consistency** — compare `wiki/index.md` (and all pagination pages) against actual `wiki/topics/` and `wiki/archives/` files:
- File exists but missing from index → add entry with `(no summary)` placeholder
- Index entry points to nonexistent file → mark as `[MISSING]` in the index

**Internal links** — for every markdown link in topic/archive files (body + Sources metadata), excluding Raw field links:
- Target does not exist → search `wiki/topics/` for a file with the same name elsewhere
  - Exactly one match → fix the path
  - Zero or multiple matches → report to the user

**Raw references** — every link in a Raw field must point to an existing `wiki/raw/` file:
- Target does not exist → search `wiki/raw/` for the file elsewhere
  - Exactly one match → fix the path
  - Zero or multiple matches → report to the user

**See Also** — within each topic directory:
- Add obviously missing cross-references between related articles
- Remove links to deleted files

#### Heuristic Checks (report only)

These rely on your judgment. Report findings without auto-fixing:

- Factual contradictions across articles
- Outdated claims superseded by newer sources
- Missing conflict annotations where sources disagree
- Orphan pages with no inbound links from other wiki articles
- Missing cross-topic references
- Concepts frequently mentioned but lacking a dedicated page
- Archive pages whose cited source articles have been substantially updated since archival
- Raw files that have never been processed into a topic article

#### Post-Lint

Append to `wiki/log.md`:
```
## [YYYY-MM-DD] lint | <N> issues found, <M> auto-fixed
- <brief list of issues found>
```
</lint>

---

## Index Pagination

<pagination>
The main `wiki/index.md` should stay navigable. When it grows beyond ~200 entries:

1. Move the oldest/least-active topic sections to `wiki/index-1.md`
2. Add a navigation header to both files:
   ```markdown
   > Pages: [index](index.md) | [index-1](index-1.md) | [index-2](index-2.md)
   ```
3. Continue paginating as `index-2.md`, `index-3.md`, etc.
4. Always keep the most active topics in the main `index.md`
</pagination>

---

## Conventions

<conventions>
- Standard markdown with relative links throughout.
- `wiki/topics/` supports up to two levels of subdirectories (topic/sub-topic). No deeper nesting.
- `wiki/raw/` supports type/topic nesting at any reasonable depth.
- Today's date for log entries, Collected dates, and Archived dates.
- Updated dates reflect when the article's knowledge content last changed, not filesystem timestamps.
- Published dates come from the source — use `Unknown` when unavailable.
- Inside wiki files, all markdown links use paths relative to the current file.
- In conversation output, use project-root-relative paths (e.g., `wiki/topics/attention/flash-attn.md`).
- Ingest updates both `wiki/index.md` and `wiki/log.md`.
- Archive (from Query) updates both.
- Correct updates both.
- Lint updates `wiki/log.md` (and `wiki/index.md` only when auto-fixing entries).
- Plain queries do not write any files.
- Binary files in `raw/` are referenced by their processed `.md` counterpart in `topics/`.
</conventions>

---

## Templates

All templates live in `references/` relative to this SKILL.md file. Read them when you need the exact format:

| Template | Purpose |
|----------|---------|
| `references/raw-template.md` | Metadata header for text sources saved to `raw/` |
| `references/topic-template.md` | Knowledge article format for `topics/` |
| `references/archive-template.md` | Archived query result format for `archives/` |
| `references/index-template.md` | Index page format and pagination |
| `references/processed-binary-template.md` | Template for processed binary files (PDFs, etc.) |
