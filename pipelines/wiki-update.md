# Update

<info>
Keep the wiki up to date without re-saving the original source. Default to adding newer knowledge; only fix claims when the knowledge has changed or an earlier claim is clearly wrong.
</info>

<critical>
1. Do not re-save raw material unless the task actually adds a new source.
2. Default to add-only edits. Use in-place fix only when the old claim is outdated, replaced, or wrong.
3. Update dependent topic pages when the changed fact affects them.
4. Only touch the index when page summaries or coverage change in a meaningful way.
</critical>

<tool-choices>
1. Read the target page and any directly dependent topic pages before editing.
2. Prefer small edits over wide rewrites.
3. Reuse the existing fix pattern exactly when fixing a claim that truly needs correction.
4. Source priority: user files/text → inbuilt search → `exa`/`tavily`/`linkup` (equal priority, use pagination) → `arxiv`/`alphaxiv` → others. For code, use GitHub search: `https://github.com/search?q=<query>+language%3A<lang>&type=repositories&s=updated&o=desc`.
</tool-choices>

<step>
1. Find the relevant page or pages in `./wiki/topics/`.
2. Read enough surrounding context to place the new information in the correct section.
3. Decide whether the change is add or fix. Add by default; fix only if the old claim is no longer true or was wrong.
4. For fixes, write: `~~Old claim~~ **Corrected [YYYY-MM-DD]:** New info. (Source: <attribution>)`.
5. For additions, add the new material to the appropriate section with credit.
6. Update topic pages that depend on the changed claim when the new information affects them in a meaningful way.
7. Update `./wiki/index.md` if the page summary or scope has changed.
8. Add a log entry:
   `## [YYYY-MM-DD HH:MM] update | <article> - <reason>`
   `- <brief description of changes>`
</step>
