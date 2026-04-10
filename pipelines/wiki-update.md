# Pipeline: Update

Append to or correct existing knowledge without re-ingesting.

<steps>
## Steps

1. Locate target article(s) in `wiki/topics/`
2. Apply change:

   **Correction** — strikethrough old claim, add attribution:
   ```markdown
   ~~Previous claim~~ **Corrected:** New info. (Source: <attribution>)
   ```

   **Append** — add new information to appropriate section.

3. Run cascade update logic — other articles referencing changed content may need updating
4. Update `wiki/index.md` if summary changed
5. Append to `wiki/log.md`:
   ```
   ## update | <article title> — <brief reason>
   ```
</steps>
