---
name: mwp-read-transcripts
description: Download and read My Weird Prompts podcast transcripts, either one episode, the latest few, or the whole archive in bulk. Use when you need what the hosts actually said, to quote or summarise an episode, or to analyse the show's text. Read-only, no sign-up.
license: CC-BY-4.0
metadata:
  publisher: My Weird Prompts (https://myweirdprompts.com)
  version: "1.0"
---

# Read My Weird Prompts transcripts

Each transcript opens with Daniel's prompt, followed by the hosts' conversation
with one `Speaker: text` turn per paragraph. Most hosts are Corn and Herman.
Hilbert, the producer, often has a turn. Expect 20,000+ characters per episode.

You need the episode's **slug**. If you don't have it, use `mwp-find-episodes`
first, or start from the latest episodes as shown below.

## One episode

| Want | Request |
|---|---|
| Plain text, cheapest | `GET https://myweirdprompts.com/transcripts/{slug}.txt` |
| Full record (title, date, tags, show notes, audio URL, transcript) | `GET https://myweirdprompts.com/api/episodes/{slug}.json` → `transcript` |
| By MCP | `get_transcript` on `https://myweirdprompts.com/mcp` (accepts `slug` or `episode_number`) |

`/transcripts/{slug}.txt` is a static file written at site build. An episode
published minutes ago can return 404 there. In that case use the JSON
endpoint, which falls back to the content catalogue.

## Latest N

    curl -s 'https://myweirdprompts.com/api/episodes.json?limit=3' \
      | jq -r '.episodes[].slug' \
      | while read s; do
          echo "=== $s"; curl -s "https://myweirdprompts.com/transcripts/$s.txt"
        done

## Whole archive

Don't crawl the site for this. Two public Hugging Face datasets are synced
daily:

- `My-Weird-Prompts/transcripts`: the text corpus.
  https://huggingface.co/datasets/My-Weird-Prompts/transcripts
- `My-Weird-Prompts/episodes`: metadata (dates, tags, durations, audio URLs).
  https://huggingface.co/datasets/My-Weird-Prompts/episodes

They lag the site by up to a day.

## Rules

- Quote sparingly and link the episode
  (`https://myweirdprompts.com/episode/{slug}/`).
- The hosts are AI and can be wrong. Don't present their claims as verified fact.
- Daniel's full prompt text is mostly not published. Don't invent it. Use what
  the transcript's opening says.
- License CC BY 4.0. Attribute as "My Weird Prompts by Daniel Rosehill" and
  note that the content is AI-generated.

More: https://myweirdprompts.com/agents/
