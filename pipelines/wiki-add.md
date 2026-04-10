# Add

Source (URL, file, paste) → ./raw/ → ./topics/. Always both phases.

<fetch>
## Fetch → ./raw/

1. Read `./wiki/index.md` to find semantically related or overlapping topics.
2. **Related topic found** → use that topic name for this source.
   **No related topic** → pick a new topic slug, create `./raw/<topic>/`.
3. Acquire via available tools. Unreachable → ask user to paste.
4. Save to `./raw/<topic>/<type>/<slug>.md`
   - Type: `articles/`, `papers/`, `books/`, `conversations/`, `other/`
   - Kebab-case slug from title, max 60 chars. Duplicates? → `-2.md`
   - Published unknown? → metadata `Unknown`
   - Header per `../references/raw-template.md`. Preserve original text, clean noise only. Do not rewrite opinions unless false.
   - Binaries: save directly to `./raw/<topic>/<type>/`, no metadata header.
</fetch>

<process>
## Process → ./topics/

- **Related topic exists** → append to matching article in `./topics/<topic>/`, add source to metadata
- **No related article** → new article, name after concept not raw file
- **Binary** → `<filename>.md` linking to raw. Use `../references/processed-binary-template.md`
- **Spans topics** → most relevant dir + See Also cross-refs

One source may both merge and spawn. Conflicts: annotate inline with attribution, cross-link.

Format: `../references/topic-template.md`
</process>

<cascade>
## Cascade

Scan same topic dir + index entries in other topics. Update materially affected articles. Never cascade archives.
</cascade>

<post>
## Post-Add

1. Update `./wiki/index.md` add/update entries per `../references/index-template.md`. New topic sections get one-line description. Paginate at ~200.
2. Append to `wiki/log(?-*).md`:
   ```
   ## [YYYY-MM-DD HH:MM] add | <article title>
   - Source: <raw path>
   - Updated: <cascade article>
   ```
</post>
