# Skill: Jarvis Setup

## What Jarvis is
Jarvis is Arnold's personal Telegram bot ecosystem running on n8nitro (cloud n8n at arso.apps.n8nitro.com). It handles automated briefings, solar alerts, market data, and now two-way AI conversation.

## Bots
| Bot | Purpose |
|---|---|
| Jarvis | Main bot — briefings, alerts, AI replies |
| Bookman | Standalone audio-to-text tool. Not integrated with Jarvis. |

All bots send to Telegram chat ID: **1534181531**

## Core workflows on n8nitro
| Workflow | ID | Schedule | Status |
|---|---|---|---|
| Daily News Briefing via Jarvis v2 | Ccg4toEohjnjIO9CnjIxC | 6:00 AM AEST daily | Active |
| Solar Monitor Phase 1 v4 | JTxWT1L5R2ZjHmUJBurXB | Hourly + noon + 7PM | Active |
| Market Radar AI — Daily Pipeline | oNkmYqyVH4AXyPIp | 11:00 AM AEST | Active |
| Market Radar AI — Briefing | FiqGob5TugrIiQei | 11:30 AM AEST | Active |
| Jarvis — Message Relay | 0ZjKy7iDDwVjdbwz | Webhook | Active |
| Jarvis — Skill-Aware Agent | X65GWAvhKfDMBB7L | Telegram trigger | Active |
| Gmail Auto-Labelling Agent | IoLPJDjOhDMfXg92 | Trigger | Active |
| Workflow Error Monitor | QAZey9LLwDzDwDCy | Error handler | Active |

## Message Relay
Any system can send a Telegram message to Jarvis via HTTP POST:
- Endpoint: `/webhook/jarvis-relay`
- Body: `{ "message": "your text here" }`
- Used by: Claude daily dashboard, scheduled tasks

## Skill-Aware Agent
Workflow ID: X65GWAvhKfDMBB7L

Two modes:
- **Mode 1** — plain message to Jarvis → Claude Sonnet 4.6 answers with Jarvis context
- **Mode 2** — `/skill [name] [question]` → fetches skill file from GitHub → Claude answers with loaded context

Skill files live at: `github.com/HFBDGD/jarvis-skills/skills/[name].md`

Claude model: `claude-sonnet-4-6`
Max tokens per reply: 1024

## Error handling
All active workflows route errors to the **Workflow Error Monitor** (ID: QAZey9LLwDzDwDCy) which logs to Supabase and sends a Telegram alert.

## AI stack
| Task | Model |
|---|---|
| Daily news briefing | Gemini 2.5 Flash Lite (with Google Search grounding) |
| Market analysis (JSON) | Gemini 2.5 Flash |
| Market briefing (prose) | Claude Sonnet 4.6 |
| Jarvis AI replies | Claude Sonnet 4.6 |

## API budgets
- Claude API: ~$5/month cap
- Gemini API: ~$5/month cap
- Total infrastructure ceiling: ~$30/month

## Credentials note
Gemini API keys are hardcoded in workflow nodes — n8n's credential types don't work for Growatt or Gemini query-param auth. This is intentional, not an oversight.
