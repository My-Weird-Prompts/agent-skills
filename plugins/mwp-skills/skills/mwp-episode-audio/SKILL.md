---
name: mwp-episode-audio
description: Get the audio for My Weird Prompts podcast episodes, either a direct audio file link (.m4a) for one episode or the RSS feeds to subscribe to. Use when someone wants to listen to, download, embed or subscribe to the show or a specific episode.
license: CC-BY-4.0
metadata:
  publisher: My Weird Prompts (https://myweirdprompts.com)
  version: "1.0"
---

# My Weird Prompts audio and feeds

## One episode's audio

Find the slug (use `mwp-find-episodes`), then:

    curl -s https://myweirdprompts.com/api/episodes/{slug}.json \
      | jq '{title, url, podcastAudioUrl, podcastDuration}'

`podcastAudioUrl` is a direct, downloadable audio file (AAC in .m4a; older episodes may be .mp3). The list endpoint
(`/api/episodes.json`) carries it too, so you can get audio for the latest N in
a single request:

    curl -s 'https://myweirdprompts.com/api/episodes.json?limit=5' \
      | jq -r '.episodes[] | "\(.title)\t\(.podcastAudioUrl)"'

To hand an episode to a person, give them the episode page
(`https://myweirdprompts.com/episode/{slug}/`). It has a player, show notes and
transcript. Give that audio link when they ask for a file.

## Subscribe

| Feed | URL |
|---|---|
| Main podcast RSS (every episode) | `https://myweirdprompts.com/feed.xml` |
| One channel (topic area) | `https://myweirdprompts.com/channel/{id}/feed.xml`. Channel ids are listed at `/channels/` |
| Directory of all feeds | `https://myweirdprompts.com/feeds.json` |
| Spotify | https://myweirdprompts.com/spotify |
| 24/7 radio stream (MWP FM) | https://stream.myweirdprompts.com |

## Rules

- Episodes run about 20–30 minutes. If you are downloading many, use the
  Hugging Face `My-Weird-Prompts/episodes` dataset for the URL list rather than
  paging the API.
- Audio is AI-generated speech. License CC BY 4.0. Attribute as "My Weird
  Prompts by Daniel Rosehill".

More: https://myweirdprompts.com/agents/
