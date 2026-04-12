# Check

<info>
Keep the wiki working well as it grows. Fix clear structural issues automatically, then report higher-level knowledge problems that need judgment or more research.
</info>

<critical>
1. Auto-fix only clear structural issues with one safe fix.
2. Do not delete missing or broken entries; keep proof of the issue and mark problems clearly.
3. Report knowledge-quality issues even when no safe automatic fix exists.
4. Treat archives as saved snapshots; report archive drift but do not rewrite archived content.
</critical>

<tool-choices>
1. Start from the index and linked topic pages before scanning widely.
2. When a broken link has exactly one clear replacement, fix it; otherwise report it.
3. Focus auto-fixes on index entries, internal links, raw links, and obvious `See Also` gaps.
</tool-choices>

<step>
1. Check index coverage. If an index entry points to a missing file, add `(no summary)` or mark the entry `[MISSING]`; do not delete it.
2. Check internal topic links. If a broken link has one unique filename match, fix it; otherwise report it.
3. Check raw references. If a broken `Raw` link has one unique match under `./wiki/raw/<topic>/<type>/`, fix it; otherwise report it.
4. Review `See Also` links. Add obvious missing cross-links and remove dead ones when the replacement is clear.
5. Report only: factual conflicts, old claims replaced by newer sources, missing conflict notes, orphan pages, repeated concepts lacking dedicated pages, archives citing heavily updated topic pages, and unprocessed raw files.
6. Add a log entry:
   `## [YYYY-MM-DD HH:MM] check | <N> issues found, <M> auto-fixed`
   `- <brief list of issues>`
</step>
