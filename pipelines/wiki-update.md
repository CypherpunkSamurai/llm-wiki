# Update

<info>
Update keeps the wiki current without re-ingesting the original source. Use it to correct claims, append newer knowledge, and preserve the wiki as a maintained, compounding artifact.
</info>

<critical>
1. Do not re-ingest raw material unless the task actually introduces a new source.
2. Preserve history by correcting in place with explicit dated attribution instead of silently replacing claims.
3. Cascade updates to dependent topic pages when the changed fact affects them.
4. Only touch the index when article summaries or coverage materially change.
</critical>

<tool-choices>
1. Read the target article and any directly dependent topic pages before editing.
2. Prefer narrow edits over broad rewrites.
3. Reuse the existing correction pattern exactly when fixing a claim.
</tool-choices>

<step>
1. Locate the relevant article or articles in `./wiki/topics/`.
2. Read enough surrounding context to place the new information in the correct section.
3. For corrections, write: `~~Old claim~~ **Corrected [YYYY-MM-DD]:** New info. (Source: <attribution>)`.
4. For additions, append the new material to the appropriate section with attribution.
5. Update topic pages that depend on the changed claim when the new information materially affects them.
6. Update `./wiki/index.md` if the article summary or scope has changed.
7. Append a log entry:
   `## [YYYY-MM-DD HH:MM] update | <article> - <reason>`
   `- <brief description of changes>`
</step>
