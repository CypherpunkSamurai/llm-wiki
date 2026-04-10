# Update

Append or correct existing knowledge without re-ingesting.

<steps>
1. Locate article(s) in `./wiki/topics/`
2. **Correct:** `~~Old claim~~ **Corrected [YYYY-MM-DD]:** New info. (Source: <attribution>)`
   **Append:** add to appropriate section.
3. Cascade — update articles referencing changed content
4. Update index if summary changed
5. Add Log `wiki/log(?\-[0-9]*\.md)`:
   ```
   ## [YYYY-MM-DD HH:MM] update | <article> — <reason>
   - <brief description of changes>
   ```
</steps>
