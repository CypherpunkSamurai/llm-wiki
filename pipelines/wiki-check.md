# Pipeline: Check

Quality checks on the wiki.

<autofix>
## Auto-fix

Fix these automatically:

- **Index consistency** — file exists but missing from index → add with `(no summary)`. Index entry points to missing file → mark `[MISSING]`.
- **Broken internal links** — search wiki for moved files. Unique match → fix path. Zero/multiple → report.
- **Broken raw references** — same approach for Raw field links.
- **See Also** — add obvious missing cross-refs within topic dirs. Remove dead links.
</autofix>

<report>
## Report Only

Flag but don't auto-fix:

- Factual contradictions across articles
- Outdated claims superseded by newer sources
- Orphan pages with no inbound links
- Raw files never processed into a topic article
</report>

<post>
## Post-Check

Append to `wiki/log.md`:
```
## check | <N> issues found, <M> auto-fixed
- <brief list of issues>
```
</post>
