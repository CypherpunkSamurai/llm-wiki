# Add

<info>
Turn one source into durable knowledge. Preserve the raw source first, then integrate its meaning into the wiki so future queries reuse accumulated synthesis instead of rediscovering it.
</info>

<critical>
1. Always do both phases: raw capture, then topic processing.
2. Raw files are immutable. Save the source as-is before summarizing or restructuring it.
3. Reuse existing topics when the source materially overlaps them; create a new topic only when the index has no strong fit.
4. Default to append-only integration. Correct existing claims only when the source shows the old claim is outdated, superseded, or false.
5. Never cascade changes into `./wiki/archives/`.
</critical>

<tool-choices>
1. Read `./wiki/index*.md` first; avoid broad file scans until the index is insufficient.
2. Detect download tools once and reuse the result.
3. Prefer `aria2c`, then `curl` or `wget`, then `python` or `python3`, then `node` or `bun`, then `powershell` or `pwsh`.
4. If a URL is unreachable, ask the user to paste or provide the file instead of inventing content.
5. Use `../references/raw-template.md`, `../references/topic-template.md`, `../references/processed-binary-template.md`, and `../references/index-template.md` only when you need exact output shape.
</tool-choices>

<step>
1. Read `./wiki/index*.md` to find the closest existing topic or confirm a new topic is needed.
2. Choose a topic slug. If related material already exists, reuse that topic; otherwise create a new topic path under `./wiki/raw/` and `./wiki/topics/`.
3. Capture the source into `./wiki/raw/<topic>/<type>/` using a kebab-case slug capped at 60 chars. Text uses `<slug>.md`; binaries keep their native extension. Duplicates get `-2`, `-3`, and so on.
4. For text raw files, use `../references/raw-template.md`, preserve source meaning exactly, and only clean obvious formatting noise. For binaries, save the file as-is with no metadata wrapper.
5. Process the raw source into `./wiki/topics/<topic>/`. Merge into an existing article when the concept already exists; otherwise create a concept-oriented article name rather than mirroring the raw filename.
6. If the raw file is binary, create a processed companion markdown page with `../references/processed-binary-template.md`.
7. Scan the same topic and nearby indexed topics for materially affected pages. Append by default, correct only when the new source invalidates or updates an older claim, and add cross-links when the source spans multiple topics.
8. Update `./wiki/index.md` with added or changed topic pages. Add a one-line topic description when creating a new topic section. Paginate at about 200 entries.
9. Append a log entry:
   `## [YYYY-MM-DD HH:MM] add | <article title>`
   `- Source: <raw path>`
   `- Updated: <cascade article>`
</step>
