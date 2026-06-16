# wabot

Multi-persona AI bot system for WhatsApp group chats, orchestrated with [n8n](https://n8n.io/).

wabot runs several AI personas inside the same WhatsApp group at once. Each bot has its own personality, its own way of reacting to the other bots, and its own logic for deciding when to speak — so the group feels like a small cast of characters rather than a single assistant replying to messages.

This repo is a showcase of the project's design and workflows rather than a plug-and-play template — credentials, live endpoints, and group-specific config aren't included.

## Features

- **Multiple personas, one group** — distinct bots with their own tone and personality coexist in a single WhatsApp chat.
- **Bot-to-bot awareness** — bots can react to and reference what another bot just said, not just what humans say.
- **Response gating** — an emotion-intensity classifier scores each incoming message and only triggers a bot reply above a configurable threshold, keeping the system from responding to every low-signal message.
- **Adjustable sensitivity** — the response threshold can be tuned on the fly via a slash command, stored in an n8n data table.
- **Workflow-based** — built entirely on n8n workflows, so logic is visual and easy to reason about without a full backend codebase.

## Architecture

```
WhatsApp group message
        │
        ▼
Emotion Intensity Classifier  (lightweight LLM, scores message 0–1)
        │
        ▼
   score > threshold?  ──No──▶ no response
        │ Yes
        ▼
  Bot-specific n8n workflow
        │
        ▼
   LLM generates reply in that bot's voice
        │
        ▼
   Reply sent back to the WhatsApp group
```

## Tech Stack

| Component | Purpose |
|---|---|
| [n8n](https://n8n.io/) | Workflow orchestration for each bot |
| WhatsApp | Group chat delivery platform |
| Gemini / Gemma | Primary LLM backbone for bot responses |

## How It Works

1. A message lands in the WhatsApp group.
2. The emotion-intensity classifier scores it from 0 to 1.
3. If the score clears the threshold (default 0.5, adjustable per group via slash command), the matching bot's n8n workflow fires.
4. The bot's LLM generates a reply in character, aware of what the other bots have said, and posts it back to the group.

## Roadmap

- [ ] Finalize and ship the remaining bot persona(s)
- [ ] Expand bot-to-bot interaction logic
- [ ] General tuning based on real group usage

## License

This project is shared for demonstration purposes. Reach out before reusing any part of it commercially.
