# Check

<info>
Check keeps the wiki healthy as it grows. Fix safe structural issues automatically, then surface higher-level knowledge problems that require judgment or more research.
</info>

<critical>
1. Auto-fix only clear structural issues with a single safe resolution.
2. Do not delete missing or broken entries; preserve evidence and mark problems visibly.
3. Report knowledge-quality issues even when no safe automatic fix exists.
4. Treat archives as snapshots; report archive drift but do not rewrite archived content.
</critical>

<tool-choices>
1. Start from the index and linked topic pages before scanning broadly.
2. When a broken link has exactly one clear replacement, fix it; otherwise report it.
3. Focus auto-fixes on index entries, internal links, raw links, and obvious `See Also` gaps.
</tool-choices>

<step>
1. Check index coverage. If an index entry points to a missing file, add `(no summary)` or mark the entry `[MISSING]`; do not delete it.
2. Check internal topic links. If a broken link has one unique filename match, fix it; otherwise report it.
3. Check raw references. If a broken `Raw` link has one unique match under `./wiki/raw/<topic>/<type>/`, fix it; otherwise report it.
4. Review `See Also` links. Add obvious missing cross-links and remove dead ones when the replacement is clear.
5. Report only: factual contradictions, stale claims superseded by newer sources, missing conflict annotations, orphan pages, repeated concepts lacking dedicated pages, archives citing substantially updated topic pages, and unprocessed raw files.
6. Append a log entry:
   `## [YYYY-MM-DD HH:MM] check | <N> issues found, <M> auto-fixed`
   `- <brief list of issues>`
</step>
