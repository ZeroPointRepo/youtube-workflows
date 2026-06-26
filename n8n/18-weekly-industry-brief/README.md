# 18 · Weekly Industry Brief across Channels

Monitor 5–10 channels and get one executive brief every week. It pulls each channel's latest uploads, keeps the videos published in the last 7 days, transcribes them, and has an LLM synthesize a single Markdown brief — key developments, themes, and what to watch — posted to Slack. For marketers and analysts tracking an industry.

```
Every Week → Set Channel List → Get Latest Videos (TranscriptAPI) → Collect New Videos → Keep Top 10
           → Get Transcript (TranscriptAPI) → Aggregate Brief Input → Write Industry Brief (LLM) → Send Brief
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every Week** | Schedule Trigger | Runs weekly (every 7 days at 08:00). |
| **Set Channel List** | Code | The list of channels to monitor (edit the array). |
| **Get Latest Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/channel/latest` per channel — **free**. |
| **Collect New Videos** | Code | Flattens to videos published in the last 7 days (precise ISO timestamps). |
| **Keep Top 10** | Limit | Caps how many videos to transcribe (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Aggregate Brief Input** | Code | Combines the transcripts into one bounded corpus. |
| **Write Industry Brief (LLM)** | HTTP Request | Synthesizes the executive brief in Markdown. |
| **Send Brief** | HTTP Request | Posts the brief to Slack (`chat.postMessage`). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.
- A **Slack** bot token (`chat:write`) + channel ID (or swap for email).

## Setup

1. **Import** `weekly-industry-brief.json`.
2. Create three **Header Auth** credentials and attach each to its node:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
   | `Slack - Authorization Bearer` | `Authorization` | `Bearer YOUR_SLACK_BOT_TOKEN` |
3. Open **Set Channel List** and replace the placeholder channels with your own (add as many as you like).
4. In **Send Brief**, replace `REPLACE_WITH_SLACK_CHANNEL_ID` with your channel ID.
5. **Activate** the workflow (or click **Test workflow** to generate one now).

## Customizing

- **Window:** change `WINDOW_DAYS` in **Collect New Videos** to match your cadence.
- **Volume:** raise `maxItems` on **Keep Top 10** for broader coverage (more credits).
- **Email instead of Slack:** swap **Send Brief** for an email node.

## Cost

Per weekly run = **0 credits** for the channel checks (free) + **1 credit per transcribed video** (capped by **Keep Top 10**) + one LLM call. A quiet week with no new videos posts nothing.

## Notes

- Recency uses the exact ISO publish timestamps from `channel/latest`, so the 7-day window is precise.
- The credit budget is bounded by **Keep Top 10**; raise or lower it to trade coverage for cost.
