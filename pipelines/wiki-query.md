# Query

<info>
Answer from the accumulated wiki first. Reuse maintained synthesis and citations instead of rebuilding the answer from scratch unless the wiki is missing necessary knowledge.
</info>

<critical>
1. Prefer wiki content over model memory when the wiki covers the question.
2. Read the index before opening topic pages.
3. Cite topic pages in the response.
4. Do not write files unless the user explicitly asks to save the answer.
5. Archives are frozen snapshots. Never merge archived answers back into topic pages automatically.
</critical>

<tool-choices>
1. Start with `./wiki/index*.md`, then read only the pages needed to answer well.
2. Use `../references/archive-template.md` only when the user asks to save the result.
3. Prefer concise citations in the form `[[Title]](./wiki/topics/<topic>/<article>.md)`.
</tool-choices>

<step>
1. Read `./wiki/index*.md` to locate the most relevant topic pages.
2. Read those topic pages and synthesize the answer from accumulated knowledge.
3. Prefer the wiki over training knowledge; if the wiki is incomplete, say so explicitly.
4. Answer in conversation with citations to the topic pages you relied on.
5. If the user asks to save the answer, create a new page in `./wiki/archives/` using `../references/archive-template.md`, keep it independent from topics, rewrite project-root paths to file-relative paths, update the index with an `[Archived]` summary, and append:
   `## [YYYY-MM-DD HH:MM] query | Archived: <page title>`
</step>
