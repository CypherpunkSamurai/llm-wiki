# Pipeline: Query

Search wiki and answer from accumulated knowledge.

<steps>
## Steps

1. Read `wiki/index*.md` to locate relevant articles
2. Read those articles, synthesize an answer
3. Prefer wiki content over training knowledge. Cite: `[Title](wiki/topics/<topic>/<article>.md)`
4. Output in conversation. No file writes unless asked.
</steps>

<archive>
## Archive

When user explicitly asks to save the answer:

1. Write to `wiki/archives/` per `../references/archive-template.md`
   - Always new page, never merge into existing articles
   - Sources: links to cited topic articles
   - No Raw field
2. Update `wiki/index.md` — prefix summary with `[Archived]`
3. Append to `wiki/log.md`:
   ```
   ## query | Archived: <page title>
   ```
</archive>
