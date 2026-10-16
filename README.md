# My Weird Prompts — Claude Code plugins

The official [Claude Code](https://claude.com/claude-code) plugin marketplace for
[My Weird Prompts](https://myweirdprompts.com), an AI-generated podcast.

## Install

Inside Claude Code:

```
/plugin marketplace add My-Weird-Prompts/claude-plugins
/plugin install mwp-skills@my-weird-prompts
```

Update later with `/plugin marketplace update my-weird-prompts`.

## Plugins

### `mwp-skills`

| Skill | What it does |
|---|---|
| `mwp-find-episodes` | Find episodes by topic, keyword, tag, episode number or recency. |
| `mwp-read-transcripts` | Read one transcript, the latest few, or the whole archive in bulk. |
| `mwp-episode-audio` | Get an episode's audio file or the RSS feeds. |
| `mwp-send-prompt` | Help someone shape a question for the show and send it in (with their go-ahead). |

All read-only skills use the public site and need no account. Sending a prompt
goes to a person, who picks which prompts become episodes.

## How this repo is maintained

The skills are authored in the website (`website/public/.well-known/agent-skills/`
in the show's monorepo) and published at
<https://myweirdprompts.com/.well-known/agent-skills/index.json>. **Don't edit the
`SKILL.md` files here** — they are overwritten.
`.github/workflows/sync-skills.yml` runs `scripts/sync_skills.py` daily (and on
demand from the Actions tab): it downloads every skill in the index, checks its
sha256 against the index, writes it under `plugins/mwp-skills/skills/`, deletes
skills the index no longer lists, and bumps the plugin's patch version when
anything changed.

## License

Skills: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — "My Weird
Prompts by Daniel Rosehill".
