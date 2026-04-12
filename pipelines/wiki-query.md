# Query

<info>
Answer from the existing wiki first. Reuse saved knowledge and citations instead of building the answer from scratch unless the wiki lacks needed knowledge.
</info>

<critical>
1. Use wiki content over model memory when the wiki covers the question.
2. Read the index before opening topic pages.
3. Cite topic pages in the response.
4. Do not write files unless the user explicitly asks to save the answer.
5. Archives are saved snapshots. Never merge archived answers back into topic pages automatically.
</critical>

<tool-choices>
1. Start with `./wiki/index*.md`, then read only the pages needed to answer well.
2. Use `../references/archive-template.md` only when the user asks to save the result.
3. Prefer short citations in the form `[[Title]](./wiki/topics/<topic>/<article>.md)`.
</tool-choices>

<step>
1. Read `./wiki/index*.md` to find the most relevant topic pages.
2. Read those topic pages and build the answer from saved knowledge.
3. Use the wiki over training knowledge; if the wiki is incomplete, say so clearly.
4. Answer in chat with citations to the topic pages you used.
5. If the user asks to save the answer, create a new page in `./wiki/archives/` using `../references/archive-template.md`, keep it separate from topics, rewrite project-root paths to file-relative paths, update the index with an `[Archived]` summary, and add:
   `## [YYYY-MM-DD HH:MM] query | Archived: <page title>`
</step>
