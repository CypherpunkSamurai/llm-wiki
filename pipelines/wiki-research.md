# Pipeline: Research

User wants to explore a topic, potentially exhaustively. Follows tangents.

<flow>
## Flow

1. **Check existing knowledge** — fuzzy grep `wiki/index*.md` for the topic and related terms
2. **Branch based on results:**

### Topic exists
- Read existing article(s)
- Identify gaps, outdated info, or shallow coverage
- Research to fill gaps using available tools (web search, URL fetch, etc.)
- For each new source found: create raw reference first (`raw/`), then append findings to existing topic article
- Never replace existing content — only append or correct with attribution
- Follow tangents: if research reveals related concepts not yet in the wiki, create new raw + topic entries for those too

### Topic doesn't exist
- Research the topic using available tools
- Create raw reference(s) for each source found
- Process into new topic article(s)
- Follow tangents: if the research branches into related areas of interest, capture those as separate raw + topic entries

In both cases: **raw references always come before topic modifications.**
</flow>

<tangents>
## Following Tangents

When researching, don't suppress tangential discoveries. If a source leads to a related concept worth capturing:

1. Note the tangent
2. Finish the primary topic processing
3. Create raw + topic entries for the tangent
4. Cross-link via See Also

This is how the knowledge base grows organically — each research session can spawn multiple interconnected articles.
</tangents>

<post>
## Post-Research

1. Update `wiki/index.md` for all new/modified articles
2. Append to `wiki/log.md`:
   ```
   ## research | <primary topic>
   - Added: <new article title>
   - Updated: <updated article title>
   - Tangent: <tangent article title>
   ```
</post>
