# Add

<info>
Save a source into the wiki. First save the raw source as-is, then turn its meaning into wiki pages so future queries can reuse the knowledge.
</info>

<critical>
1. Always do both steps: save raw source, then process it into a topic.
2. Raw files never change. Save the source as-is before summarizing or changing it.
3. Reuse existing topics when the source fits them well; make a new topic only when no good fit exists.
4. Default to add-only updates. Fix old claims only when the source shows the old claim is outdated, replaced, or wrong.
5. Never change files in `./wiki/archives/`.
</critical>

<tool-choices>
1. Read `./wiki/index*.md` first; avoid wide file scans unless the index is not enough.
2. Find download tools once and reuse the result.
3. Try `aria2c` first, then `curl` or `wget`, then `python` or `python3`, then `node` or `bun`, then `powershell` or `pwsh`.
4. If a URL does not work, ask the user to paste text or provide a file instead of making up content.
5. Use `../references/raw-template.md`, `../references/topic-template.md`, `../references/processed-binary-template.md`, and `../references/index-template.md` only when you need exact output format.
6. Source priority: user files/text → inbuilt search → `exa`/`tavily`/`linkup` (equal priority, use pagination) → `arxiv`/`alphaxiv` → others. For codebases, use GitHub search: `https://github.com/search?q=<query>+language%3A<lang>&type=repositories&s=updated&o=desc`. Reserve one-click research for when ~10 focused searches fall short.
</tool-choices>

<step>
1. Read `./wiki/index*.md` to find matching topics or confirm a new topic is needed.
2. Pick a topic name. If matching material already exists, reuse that topic; otherwise make a new topic path under `./wiki/raw/` and `./wiki/topics/`.
3. Save the source into `./wiki/raw/<topic>/<type>/` using a kebab-case name up to 60 chars. Text files use `<name>.md`; binaries keep their own extension. Duplicates get `-2`, `-3`, etc.
4. For text raw files, use `../references/raw-template.md`, keep source meaning exact, and only fix clear formatting noise. For binaries, save the file as-is with no metadata wrapper.
5. Turn the raw source into `./wiki/topics/<topic>/`. Merge into an existing page when the concept already exists; otherwise make a concept-focused page name instead of copying the raw filename.
6. If the raw file is binary, create a matching markdown page with `../references/processed-binary-template.md`.
7. Check the same topic and nearby indexed topics for affected pages. Add by default, fix only when the new source replaces or updates an older claim, and add cross-links when the source covers multiple topics.
8. Update `./wiki/index.md` with added or changed topic pages. Add a one-line topic description when making a new topic section. Split at about 200 entries.
9. Add a log entry:
   `## [YYYY-MM-DD HH:MM] add | <article title>`
   `- Source: <raw path>`
   `- Updated: <changed article>`
</step>
