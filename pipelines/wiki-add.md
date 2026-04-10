# Add

Source (URL, file, paste) → ./raw/ → ./topics/. Always both phases.

<fetch>
## Fetch → ./raw/

1. Acquire via available tools. Unreachable → ask user to paste.
2. Classify source → `./raw/` subdir: `./raw/articles/`, `./raw/books/`, `./raw/papers/`, `./raw/conversations/`, `./raw/other/`. Reuse existing subdirs when topic is close enough.
3. Save as `./raw/<type>/<optional-topic>/descriptive-slug.md`
   - Kebab-case slug from title, max 60 chars. Duplicates? → `-2.md`
   - Published unknown? → metadata `Unknown`
   - Header per `../references/raw-template.md`. Preserve original text, clean noise only. Do not rewrite opinions unless false.
   - Binaries: save directly, no metadata header.
</fetch>

<process>
## Process → ./topics/

- **Same thesis as existing** → append to article, add source to metadata
- **New concept** → new article, name after concept not raw file
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
