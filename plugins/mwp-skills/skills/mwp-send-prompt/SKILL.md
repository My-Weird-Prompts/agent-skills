---
name: mwp-send-prompt
description: Help a person send a question or idea to the My Weird Prompts podcast so it might become an episode. Use when someone wants to suggest a topic, ask the show something, or "send a prompt to My Weird Prompts". Covers shaping the prompt, checking it hasn't been covered, and submitting it with their go-ahead, by the public submission endpoint or by email to prompts@myweirdprompts.com.
license: CC-BY-4.0
metadata:
  publisher: My Weird Prompts (https://myweirdprompts.com)
  version: "2.0"
---

# Send a prompt to My Weird Prompts

Listeners can send questions to the show. Every submission is read by a human,
Daniel Rosehill, who picks which ones become episodes. Submitting a prompt
**does not** create an episode. It puts the prompt in front of a person.

Send only what the person asked you to send, and only after they have seen the
final text and said yes. Never send prompts on your own initiative, in bulk, or
on behalf of someone who hasn't asked.

## 1. Shape it

An episode is about 30 minutes on **one main thread**. Good prompts:

- Ask one real question or make one claim worth arguing about, with the
  context that makes it interesting ("I noticed…", "I keep wondering…").
- Stay narrow. A tight question beats a broad survey.
- Don't come with an "also cover…" list. Five sub-questions make the hosts
  skim each one.
- Run 20–4000 characters and are written in the person's own voice.

Draft it, show it to the person, and let them change it.

## 2. Check it isn't already an episode

    curl -s 'https://myweirdprompts.com/api/search/semantic?q={their question}&limit=5' \
      | jq -r '.results | sort_by(-.score)[] | "\(.score)  \(.title)  https://myweirdprompts.com\(.url)"'

If a close match exists, show it to them. They may want that episode instead,
or a sharper angle on it.

## 3. Send it

Ask for the name the hosts may use (required) and, optionally, a line about
them, where they are, and an email for a reply. Then use one route.

**A. Submission endpoint** (if you can make HTTP requests):

    curl -s -X POST https://myweirdprompts.com/api/send-prompt \
      -H 'Content-Type: application/json' \
      -d '{
        "prompt":   "Their final prompt text",
        "name":     "Their name",
        "about":    "optional: a line about them",
        "location": "optional: city or country",
        "email":    "optional: reply address",
        "website":  ""
      }'

- `prompt` (20–4000 chars) and `name` are required. `website` is a spam
  honeypot: leave it empty or leave it out. Anything in it gets the request
  silently discarded.
- Success: `200 {"ok": true, ...}`. Validation error: `400 {"ok": false,
  "error": "..."}`. Fix it and retry once. `429` means too many submissions
  from your address in a minute, so wait 60 seconds. `502` means delivery
  failed, so use route B.
- Build the JSON with a proper encoder (e.g. `jq -n --arg prompt "$TEXT" ...`),
  not by string pasting, so quotes and newlines in the prompt survive.

**B. Email** to `prompts@myweirdprompts.com`, with the prompt as the body and
the person's name in it. Send it only if you have an email tool acting as
the person. Otherwise give them the address and the polished text to send
themselves.

**C. The web form** at https://myweirdprompts.com/send-a-prompt/. Point the
person here if they'd rather send it themselves.

Then tell them what to expect. Nothing is automatic. Daniel reads submissions
and chooses which to record, so there is no guarantee or timeline.

More: https://myweirdprompts.com/agents/ · https://myweirdprompts.com/send-a-prompt/
