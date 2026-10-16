# My Weird Prompts — agent skills

The official agent skills for [My Weird Prompts](https://myweirdprompts.com), an
AI-generated podcast. Each skill is a single `SKILL.md` in the open
[Agent Skills](https://agentskills.io/specification) format, so it works with any
skill-aware agent.

| Skill | What it does |
|---|---|
| `mwp-find-episodes` | Find episodes by topic, keyword, tag, episode number or recency. |
| `mwp-read-transcripts` | Read one transcript, the latest few, or the whole archive in bulk. |
| `mwp-episode-audio` | Get an episode's audio file or the RSS feeds. |
| `mwp-send-prompt` | Help someone shape a question for the show and send it in (with their go-ahead). |

The read-only skills use the public site and need no account. A sent prompt goes
to a person, who picks which prompts become episodes.

The skills are in [`plugins/mwp-skills/skills/`](plugins/mwp-skills/skills/), one
folder each.

## Install

**Any agent:** copy the skill folders into your agent's skills directory. You can
also fetch them straight from the site, which lists them at
<https://myweirdprompts.com/.well-known/agent-skills/index.json>
([Agent Skills Discovery](https://github.com/cloudflare/agent-skills-discovery-rfc)
v0.2.0). For example:

```sh
SKILLS_DIR=~/.agents/skills   # your agent's skills folder
curl -s https://myweirdprompts.com/.well-known/agent-skills/index.json | jq -r '.skills[].name' | while read s; do
  mkdir -p "$SKILLS_DIR/$s"
  curl -s -o "$SKILLS_DIR/$s/SKILL.md" "https://myweirdprompts.com/.well-known/agent-skills/$s/SKILL.md"
done
```

**Plugin marketplaces:** this repo is also a plugin marketplace (`my-weird-prompts`,
plugin `mwp-skills`), which agents that support marketplaces can install and keep
updated. In Claude Code, for example:

```
/plugin marketplace add My-Weird-Prompts/agent-skills
/plugin install mwp-skills@my-weird-prompts
```

## How this repo is maintained

The skills are authored in the website (`website/public/.well-known/agent-skills/`
in the show's monorepo) and published with sha256 digests in the discovery index
above. **Don't edit the `SKILL.md` files here**, because the sync overwrites them.
`.github/workflows/sync-skills.yml` runs `scripts/sync_skills.py` every day and on
demand from the Actions tab. The script downloads every skill in the index, checks
it against its digest, and writes it under `plugins/mwp-skills/skills/`. It deletes
skills the index no longer lists, and bumps the plugin's patch version when
anything changes.

## License

Skills: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — "My Weird
Prompts by Daniel Rosehill".
