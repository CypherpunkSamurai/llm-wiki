# llm-wiki

Knowledge base skill. Give it URLs, papers, or notes and it organizes them into wiki pages with an index and changelog.

> Note: This isn't Karpathy's LLM-Wiki rather My Own Note Taking Method

## Usage

```
wiki add <url/file/text>     Save a source, extract a topic page
wiki research <topic>        Look it up, write an article
wiki query <question>        Answer from existing pages
wiki update <correction>     Fix or append to a page
wiki check                   Find broken links, orphan pages
```

## How the wiki is laid out

```
wiki/
├── index.md     # Topic entries grouped by category
├── log.md       # Append-only activity log
├── raw/         # Sources — never edited after creation
├── topics/      # Written-up knowledge pages
└── archives/    # Frozen query snapshots
```

## Rules it follows

Sources in `wiki/raw/` are immutable. The skill reads them and writes summaries into `wiki/topics/`. Pages never get deleted — they grow and get corrected with date stamps. Every action goes into `wiki/log.md` so you can trace what happened and when.

Based on My Own Note Taking Ways, and like [Karpathy LLM wiki](https://gist.githubusercontent.com/karpathy/442a6bf555914893e9891c11519de94f/raw/ac46de1ad27f92b28ac95459c782c07f6b8c964a/llm-wiki.md) pattern. Full instructions in [SKILL.md](./SKILL.md).

Maintained by [CypherpunkSamurai](https://github.com/CypherpunkSamurai).
