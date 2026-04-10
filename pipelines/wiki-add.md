# Pipeline: Add

User provides a source (URL, file, paste). Two phases, always both.

<fetch>
## Phase 1: Fetch → raw/

1. Acquire content via available tools. If unreachable, ask user to paste.
2. Classify source type → pick `raw/` subdir (`articles/`, `books/`, `papers/`, `conversations/`, `other/`). Create if missing. Optional topic nesting within type dir.
3. Save as `raw/<type>/<optional-topic>/descriptive-slug.md`
   - Slug from title, kebab-case, max 60 chars
   - Duplicate names → append numeric suffix
   - Metadata header per `../references/raw-template.md`
   - Preserve original text. Clean formatting noise only. Do not rewrite opinions unless false.
   - Binaries: save directly, no metadata header.
</fetch>

<process>
## Phase 2: Process → topics/

Determine placement:
- **Same thesis as existing article** → Append to that article. Add source to metadata.
- **New concept** → Create new article. Name after concept, not raw file.
- **Processed binary** → Create `<original-filename>.md` linking back to raw.
- **Spans topics** → Most relevant dir + See Also cross-references.

Not mutually exclusive — one source may merge into one article and spawn another.

**Conflicts:** Annotate disagreement with source attribution inline. Cross-link conflicting articles.

Article format: `../references/topic-template.md`
</process>

<cascade>
## Phase 3: Cascade

1. Scan same topic dir for affected articles
2. Scan index entries in other topics for related concepts
3. Update materially affected articles

Archives are never cascade-updated.
</cascade>

<post>
## Phase 4: Post-Add

1. Update `wiki/index.md` — add/update entries for touched articles. Format per `../references/index-template.md`. Paginate at ~200 entries.
2. Append to `wiki/log.md`:
   ```
   ## add | <primary article title>
   - Source: <raw file path>
   - Updated: <cascade-updated article title>
   ```
   Omit `- Updated:` lines when no cascade.
</post>
