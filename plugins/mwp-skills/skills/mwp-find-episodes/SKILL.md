---
name: mwp-find-episodes
description: Find episodes of the My Weird Prompts podcast by topic, keyword, phrase, tag, episode number or recency. Use when someone asks what the show has said about a subject, wants the latest episodes, or names an episode you need to locate. Read-only, no sign-up.
license: CC-BY-4.0
metadata:
  publisher: My Weird Prompts (https://myweirdprompts.com)
  version: "1.0"
---

# Find My Weird Prompts episodes

My Weird Prompts is an AI-generated podcast. Daniel Rosehill sends in short
voice prompts and the AI hosts Corn (a sloth) and Herman Poppleberry (a donkey)
discuss each one. Several thousand episodes, new ones daily. Every episode has a
**slug**, which is the key for everything else.

Everything here is public, unauthenticated and CORS-open. Base URL:
`https://myweirdprompts.com`.

## Pick the route

1. **MCP client available?** Connect `https://myweirdprompts.com/mcp`
   (Streamable HTTP, no auth) and use its tools — `search_episodes`,
   `list_episodes`, `get_episode`. Same data as below.
2. **Otherwise use plain HTTP**, as in the table.

| Need | Request |
|---|---|
| Topic, phrase or anything said in a transcript | `GET /api/search/fulltext?q={query}&limit=10` |
| A question or fuzzy idea ("why do we dream") | `GET /api/search/semantic?q={question}&limit=10` |
| Latest N episodes, newest first | `GET /api/episodes.json?limit={N}` (max 100) |
| Episode by number | `GET /api/episodes.json?episode={number}` |
| By tag / category | `GET /api/episodes.json?tag={tag}` / `?category={id}` |
| Keyword in title, description or tags only | `GET /api/episodes.json?q={text}` |
| Next page | follow `_links.next`, or add `&offset=` |

Search returns `results[]`; the list API returns `episodes[]`. Both carry
`slug` and `title`. In search results `url` is a path (`/episode/{slug}/`), so
prefix the base URL. Semantic results carry a `score` (0–1) but come back
newest first. Sort by `score` yourself if you need the best match.

### Full-text search syntax

`/api/search/fulltext` takes web-search syntax: `"quoted phrase"`, `OR`,
`-exclude`. A bare number resolves as an episode number. Operators in `q`:
`tag:`, `category:`, `type:`, `after:YYYY-MM-DD`, `before:YYYY-MM-DD`,
`has:voice` (Daniel's recorded prompt aired). Example:

    curl -s 'https://myweirdprompts.com/api/search/fulltext?q=%22sleep%20debt%22%20after:2026-01&limit=5'

Choose **semantic** for questions and paraphrases, and **full-text** for exact
words, names and phrases. If one returns nothing useful, try the other.

## Then

- To read what was said, use the `mwp-read-transcripts` skill, or
  `GET /api/episodes/{slug}.json` (full record including transcript).
- To play or download the audio, use `mwp-episode-audio`.

## Rules

- Link episodes by their `url` (`https://myweirdprompts.com/episode/{slug}/`).
- The list API is rate-limited per IP. Ask for the page size you need rather
  than walking the whole archive. For bulk work, use the Hugging Face datasets
  named in `mwp-read-transcripts`.
- The content is AI-generated. If you present a claim from an episode as fact,
  say so and check it.
- License CC BY 4.0. Attribute as "My Weird Prompts by Daniel Rosehill".

More: https://myweirdprompts.com/agents/ · https://myweirdprompts.com/llms.txt
