---
title: HuggingClaw
emoji: 🐾
colorFrom: purple
colorTo: blue
sdk: docker
pinned: false
license: mit
short_description: Deploy OpenClaw on HuggingFace Spaces
app_port: 7860
---

# HuggingClaw

**Deploy [OpenClaw](https://github.com/openclaw/openclaw) on HuggingFace Spaces** — no hardware required.

OpenClaw is a self-hosted AI assistant platform with Telegram, WhatsApp integration and a web-based Control UI. HuggingClaw packages it for one-click cloud deployment on HuggingFace Spaces.

## Why HuggingFace Spaces?

- **Zero hardware** — runs on HF's free CPU tier
- **Always online** — no need to keep your computer running
- **Persistent storage** — auto-syncs data to a HF Dataset repo
- **HTTPS built-in** — secure WebSocket connections out of the box
- **One-click deploy** — fork this Space, set secrets, done

## Quick Start

### 1. Fork this Space

Click **Duplicate this Space** on HuggingFace.

### 2. Set Secrets

Go to **Settings > Repository secrets** and add:

| Secret | Required | Description |
|--------|:--------:|-------------|
| `OPENCLAW_PASSWORD` | Recommended | Password for the Control UI (default: `huggingclaw`) |
| `HF_TOKEN` | Yes | HF Access Token with write permission (for data persistence) |
| `OPENCLAW_DATASET_REPO` | Yes | Dataset repo for backup, e.g. `your-name/openclaw-data` |
| `OPENROUTER_API_KEY` | No | [OpenRouter](https://openrouter.ai) API key for LLM access |
| `TELEGRAM_BOT_TOKEN` | No | Telegram bot token from [@BotFather](https://t.me/BotFather) |
| `TELEGRAM_BOT_NAME` | No | Telegram bot username |
| `TELEGRAM_ALLOW_USER` | No | Telegram username allowed to chat |

### 3. Open the Control UI

Visit your Space URL. Enter the password in the settings panel to connect.

## Architecture

```
HuggingFace Space (Docker)
├── OpenClaw (Node.js)        — AI assistant engine
│   ├── Control UI            — Web dashboard (port 7860)
│   ├── Telegram extension    — Bot integration
│   └── WhatsApp extension    — Messaging integration
├── sync_hf.py                — Auto-sync ~/.openclaw ↔ HF Dataset
├── dns-resolve.py            — DNS pre-resolution for WhatsApp
└── entrypoint.sh             — Startup orchestration
```

## Local Development

```bash
git clone https://huggingface.co/spaces/tao-shen/HuggingClaw
cd HuggingClaw
docker build -t huggingclaw .
docker run --rm -p 7860:7860 \
  -e OPENCLAW_PASSWORD=your-password \
  -e HF_TOKEN=hf_xxx \
  -e OPENCLAW_DATASET_REPO=your-name/openclaw-data \
  huggingclaw
```

Open `http://localhost:7860` in your browser.

## Security

- **Password-protected** — the Control UI requires a password to connect
- **Secrets stay server-side** — API keys are never exposed to the browser
- **CSP headers** — Content Security Policy restricts script/resource loading

## License

MIT
