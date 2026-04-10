# Research

Explore a topic. Follow tangents. Always create raw refs before modifying topics.

<flow>
## Flow

1. Fuzzy grep `./wiki/index*.md` for topic + related terms

**Exists →** read articles, identify gaps/outdated info, research to fill, create raw ref per source, append to existing article. Never replace — append or correct with attribution. Tangential discoveries → separate raw + topic entries.

**Doesn't exist →** research topic, create raw ref(s), process into new topic article(s). Tangents → separate entries, cross-link via See Also `[[LINK]]`.
</flow>

<post>
## Post

1. Update `./wiki/index.md` for all new/modified articles
2. Add Log `wiki/log(?\-[0-9]*\.md)`:
   ```
   ## [YYYY-MM-DD HH:MM] research | <primary topic>
   - Added: <new article>
   - Updated: <updated article>
   - Tangent: <tangent article>
   ```
</post>
