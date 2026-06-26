# 24 · Market Commentary Daily Brief

Monitor a set of finance / macro YouTube channels and get a daily, analyst-style market brief in Slack. It pulls each channel's newest videos, keeps the ones from the last day, transcribes them, and synthesizes market themes, tickers/assets mentioned, macro events, and risks — with a clear "not financial advice" disclaimer.

```
Every Day → Set Channels → Get Latest Videos (TranscriptAPI) → Collect New Videos → Keep Top 8
          → Get Transcript (TranscriptAPI) → Aggregate Brief Input → Write Market Brief (LLM) → Send Brief
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every Day** | Schedule Trigger | Runs daily at 07:00. |
| **Set Channels** | Code | Your list of finance/macro channels (edit the array). |
| **Get Latest Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/channel/latest` per channel — **free**. |
| **Collect New Videos** | Code | Keeps videos published in the last day (precise ISO timestamps). |
| **Keep Top 8** | Limit | Caps how many to transcribe (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Aggregate Brief Input** | Code | Combines transcripts into one corpus. |
| **Write Market Brief (LLM)** | HTTP Request | Writes the finance brief (themes, tickers, macro, risks + disclaimer). |
| **Send Brief** | HTTP Request | Posts to Slack (`chat.postMessage`). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.
- A **Slack** bot token (`chat:write`) + channel ID (or swap for email).

## Setup

1. **Import** `market-commentary-daily-brief.json`.
2. Create three **Header Auth** credentials and attach each to its node:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
   | `Slack - Authorization Bearer` | `Authorization` | `Bearer YOUR_SLACK_BOT_TOKEN` |
3. Edit **Set Channels** with your finance/macro channels.
4. In **Send Brief**, replace `REPLACE_WITH_SLACK_CHANNEL_ID`.
5. **Activate** the workflow.

## Customizing

- **Window/volume:** tune `WINDOW_DAYS` in **Collect New Videos** and `maxItems` in **Keep Top 8**.
- **Sections:** edit the **Write Market Brief** prompt (add sectors, FX, crypto, etc.).
- **Email instead of Slack:** swap **Send Brief** for an email node.

## Cost

Per run = **0 credits** for the channel checks (free) + **1 credit per transcribed video** (capped by **Keep Top 8**) + one LLM call.

## Notes

- **Not financial advice.** The brief is auto-generated from public video transcripts for informational purposes only; the prompt appends a disclaimer and is instructed never to invent prices or figures. Always verify before acting.
- This is the finance-specific sibling of workflow 18 (general weekly industry brief).
