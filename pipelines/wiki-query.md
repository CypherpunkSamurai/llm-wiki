# Query

Search wiki, answer from accumulated knowledge.

<steps>
1. Read `./wiki/index*.md` → locate relevant articles
2. Read articles, synthesize answer
3. Prefer wiki over training knowledge. Cite: `[[Title]](./wiki/topics/<topic>/<article>.md)`
4. Output in conversation. No file writes unless asked.
</steps>

<archive>
## Archive (only when user asks to save)

1. New page in `./wiki/archives/` per `../references/archive-template.md`. Never merge. No Raw field. Rewrite project-root paths to file-relative.
2. Update index — prefix summary with `[Archived]`
3. Log: `## [YYYY-MM-DD HH:MM] query | Archived: <page title>`
</archive>
