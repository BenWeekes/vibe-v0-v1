# Agora Conversational AI Voice Agent

A Next.js 16 app that connects to an [Agora Conversational AI](https://www.agora.io/en/products/conversational-ai/) agent with real-time voice and text chat. Click **Start Call**, talk, and see live transcripts appear in the chat panel.

## Features

- **Real-time Voice** — Full-duplex audio via Agora RTC with echo cancellation, noise suppression, and auto gain control
- **Live Transcripts** — User and agent speech appears in the chat window as it happens (via RTC stream-message)
- **Text Chat** — Type a message and send it to the agent via Agora RTM
- **Agent Visualizer** — Animated orb with states (idle, joining, listening, speaking, disconnected)
- **Customizable** — Settings panel for custom system prompt and greeting
- **Self-contained** — Next.js API routes handle token generation, agent start, and hangup — no separate backend needed

## Quick Start

### 1. Install dependencies

```bash
npm install --legacy-peer-deps
```

### 2. Configure environment

```bash
cp .env.example .env.local
```

Fill in your keys in `.env.local`:

| Variable | Required | Description |
|----------|----------|-------------|
| `APP_ID` | Yes | Agora App ID (from [Agora Console](https://console.agora.io)) |
| `APP_CERTIFICATE` | Yes | Agora App Certificate (32-char hex). Used with `APP_ID` to generate v007 tokens inline for both RTC/RTM access and Agora API auth — no separate Customer Key/Secret or npm token package needed |
| `LLM_API_KEY` | Yes | LLM provider API key (e.g. OpenAI) |
| `TTS_VENDOR` | Yes | TTS provider: `rime`, `openai`, `elevenlabs`, or `cartesia` |
| `TTS_KEY` | Yes | TTS provider API key |
| `TTS_VOICE_ID` | Yes | Voice ID (e.g. `astra` for Rime, `alloy` for OpenAI) |
| `LLM_URL` | No | LLM endpoint URL (default: OpenAI) |
| `LLM_MODEL` | No | LLM model name (default: `gpt-4o-mini`) |

### 3. Run the app

```bash
npm run dev
```

Open the app, click **Start Call**, and start talking.

## Build with AI Coding Platforms

This repo is designed to be imported into AI coding platforms. Use the prompt below and point the AI at this repo.

**Prompt:**

> Build this Agora Voice AI Agent: https://github.com/AgoraIO-Conversational-AI/vibe-code-v0 — be sure to read AGENT.md in full.

### Platform Tips

**v0 (Vercel):**
- To set environment variables: **Project** > **Settings** > **Environment Variables**
- v0 may regenerate `styles/globals.css` — that's fine, the real theme is in `app/globals.css`
- The SVG logo at `public/agora.svg` must be copied as-is — it's referenced in the header

**Lovable:**
- Uses Supabase Edge Functions instead of Next.js API routes — see the [vibe-lovable](https://github.com/AgoraIO-Conversational-AI/vibe-lovable) variant

## Architecture

```
Browser (Next.js 16 + React 19)
  │
  ├─ RTC audio ←→ Agora Conversational AI Agent
  ├─ RTC stream-message ← agent transcripts
  └─ RTM publish → text messages to agent

Next.js API Routes
  ├─ GET  /api/check-env    — validates required env vars
  ├─ POST /api/start-agent  — generates RTC+RTM tokens, calls Agora ConvoAI API
  ├─ POST /api/hangup-agent — stops the agent
  └─ GET  /api/health       — health check
```

## Tech Stack

- **Framework:** Next.js 16 with App Router and Turbopack
- **Language:** TypeScript 5
- **Runtime:** React 19
- **Styling:** Tailwind CSS 4 + shadcn/ui
- **RTC SDK:** agora-rtc-sdk-ng v4.24+
- **RTM SDK:** agora-rtm v2.2+
- **Token gen:** v007 token builder (inline, server-side)

## Local Development

For running locally without deploying, see [local.md](local.md).

## License

MIT
