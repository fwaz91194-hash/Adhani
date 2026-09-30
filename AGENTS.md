# Adhani Prayer Times Bot — Base44 Dev Environment

## What this is
A Telegram bot (Python 3.12, python-telegram-bot 22.6, FastAPI/Uvicorn, SQLite).
Not a web app — there is no frontend UI. The only HTTP surface is the FastAPI
webhook server (webhook mode) with a `/health` endpoint and `/{TOKEN}` webhook route.

## Running
```bash
docker compose -f docker-compose.base44.yml up -d --build
```
- Uses `python:3.12-slim` with the repo bind-mounted at `/app`.
- Dependencies install at container startup from `requirements.txt`.
- Runs in **webhook mode** (WEBHOOK_URL is set) so a web server listens on port 3000.
- SQLite shards live in `storage/`; logs in `logs/` — both created at runtime.

## Required secret
- `TELEGRAM_TOKEN` — from @BotFather. The app exits without it and crashes
  with an invalid token (it calls Telegram API during startup initialization).

## Modes
- **Webhook** (WEBHOOK_URL set): FastAPI on PORT, Telegram sends updates to
  `WEBHOOK_URL/TOKEN`. Used in the dev environment for the preview.
- **Polling** (WEBHOOK_URL empty): no web server, polls Telegram. Not used here.

## Health check
`GET /health` → `{"status":"ok","mode":"webhook","bot":"adhani"}`

## Preview note
The root path `/` returns 404 (no handler). The preview shows the FastAPI JSON
404 — this is expected for a Telegram bot. `/health` confirms the server is live.
