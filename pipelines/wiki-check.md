# Check

Wiki quality checks.

<autofix>
## Auto-fix

- **Index** — missing file → add `(no summary)`. Missing entry → mark `[MISSING]`, don't delete.
- **Internal links** — broken → search wiki for same filename. Unique match → fix. Else → report.
- **Raw refs** — broken Raw field link → search `./wiki/raw/`. Unique match → fix. Else → report.
- **See Also** — add obvious missing cross-refs, remove dead links.
</autofix>

<report>
## Report Only

- Factual contradictions across articles
- Outdated claims superseded by newer sources
- Missing conflict annotations
- Orphan pages (no inbound links)
- Concepts mentioned often but lacking dedicated page
- Archives citing substantially updated source articles
- Unprocessed raw files
</report>

<post>
Append Log: 
```
## [YYYY-MM-DD HH:MM] check | <N> issues found, <M> auto-fixed
- <brief list of issues>
```
</post>
