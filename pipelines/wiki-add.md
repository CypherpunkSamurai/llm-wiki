# Add

Source (URL, file, paste) → ./raw/ → ./topics/. Always both phases.

<fetch>
## Fetch → ./raw/

1. Read `./wiki/index.md` to find semantically related or overlapping topics.
2. **Related topic found** → use that topic name for this source.
   **No related topic** → pick a new topic slug, create `./raw/<topic>/`.
3. **Detect available download tools** (run once, cache result):
   ```
   where curl && where wget && where aria2c && where python && where python3 && where uv && where node && where bun && where powershell && where pwsh
   ```
   On Unix: `which curl wget aria2c python python3 uv node bun powershell pwsh 2>/dev/null`
   Use whichever tools are found. Prefer `aria2c` > `curl`/`wget` > `python`/`python3` > `node`/`bun` > `powershell`/`pwsh`.
4. **Always attempt to download/copy the raw content as-is** using available tools:
   - URLs: `curl`, `wget`, `aria2c`, `python3`, `nodejs`, `bun`, `powershell`, `pwsh`
   - PDFs/Images: Download directly via same tools
   - Raw text: Save user-provided content verbatim
   - Unreachable → ask user to paste
4. Save to `./raw/<topic>/<type>/<slug>` (binaries) or `<slug>.md` (text)
   - Type: `articles/`, `papers/`, `books/`, `conversations/`, `other/`
   - Kebab-case slug from title, max 60 chars. Duplicates? → `-2.md`
   - Published unknown? → metadata `Unknown`
   - **Text files**: Header per `../references/raw-template.md`. Preserve original text exactly, clean noise only. Do not rewrite opinions unless false.
   - **Binaries (PDFs, images, etc.)**: Save as-is to `./raw/<topic>/<type>/`, no metadata header. Raw means raw.
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
