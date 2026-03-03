# Local Development

Run the v0 voice agent locally without deploying to Vercel.

## Prerequisites

- Node.js 20+ (`nvm use 20`)
- Agora credentials (`APP_ID`, `APP_CERTIFICATE`)
- LLM API key and TTS credentials

## Setup

```bash
npm install --legacy-peer-deps
cp .env.example .env.local
```

Edit `.env.local` with your credentials, then:

```bash
npx next dev --port 8086
```

Open http://localhost:8086

## How it works

Next.js API routes handle everything server-side — no separate backend needed. The routes read credentials from `.env.local` and generate v007 Agora tokens with separate RTC and RTM UIDs.

| Route | Purpose |
|-------|---------|
| `GET /api/check-env` | Validates required env vars |
| `POST /api/start-agent` | Generates tokens, starts ConvoAI agent |
| `POST /api/hangup-agent` | Stops the agent |

## Troubleshooting

- **`crypto` errors** — You need Node.js 20+. Run `nvm use 20`.
- **Port in use** — `lsof -ti:8086 \| xargs kill`
- **No audio after connecting** — Check browser microphone permissions and DevTools console for RTC errors.
