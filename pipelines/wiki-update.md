# Update

<info>
Update keeps the wiki current without re-ingesting the original source. Default to appending newer knowledge; only correct claims when the state of knowledge has changed or an earlier claim is clearly wrong.
</info>

<critical>
1. Do not re-ingest raw material unless the task actually introduces a new source.
2. Default to append-only edits. Use in-place correction only when the old claim is outdated, superseded, or false.
3. Cascade updates to dependent topic pages when the changed fact affects them.
4. Only touch the index when article summaries or coverage materially change.
</critical>

<tool-choices>
1. Read the target article and any directly dependent topic pages before editing.
2. Prefer narrow edits over broad rewrites.
3. Reuse the existing correction pattern exactly when fixing a claim that truly needs correction.
</tool-choices>

<step>
1. Locate the relevant article or articles in `./wiki/topics/`.
2. Read enough surrounding context to place the new information in the correct section.
3. Decide whether the change is append or correction. Append by default; correct only if the old claim is no longer true or was wrong.
4. For corrections, write: `~~Old claim~~ **Corrected [YYYY-MM-DD]:** New info. (Source: <attribution>)`.
5. For additions, append the new material to the appropriate section with attribution.
6. Update topic pages that depend on the changed claim when the new information materially affects them.
7. Update `./wiki/index.md` if the article summary or scope has changed.
8. Append a log entry:
   `## [YYYY-MM-DD HH:MM] update | <article> - <reason>`
   `- <brief description of changes>`
</step>
