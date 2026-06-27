# 41 · Policy Speech Monitor

Track a government, think-tank, or policy channel and get a neutral briefing in Slack whenever it posts. It checks the channel daily, fires only on a genuinely new upload, transcribes it, and posts a factual summary — key points, stated positions, and notable quotes.

```
Every Day → Set Channel → Get Latest Videos (TranscriptAPI) → Detect New Video → Get Transcript (TranscriptAPI)
          → Build Transcript Text → Summarize Policy (LLM) → Send Summary
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every Day** | Schedule Trigger | Checks daily at 08:00. |
| **Set Channel** | Edit Fields | The policy channel (`channel` — @handle/URL/UC… ID). |
| **Get Latest Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/channel/latest` — **free**. |
| **Detect New Video** | Code | Fires once per new upload (last-seen ID in workflow static data). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches the transcript (*continue on error*). |
| **Build Transcript Text** | Code | Joins the transcript, carries title/URL/channel. |
| **Summarize Policy (LLM)** | HTTP Request | Writes a neutral Markdown briefing (summary, key points, positions, quotes). |
| **Send Summary** | HTTP Request | Posts to Slack (`chat.postMessage`). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.
- A **Slack** bot token (`chat:write`) + channel ID (or swap for email).

## Setup

1. **Import** `policy-speech-monitor.json`.
2. Create three **Header Auth** credentials and attach each to its node:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
   | `Slack - Authorization Bearer` | `Authorization` | `Bearer YOUR_SLACK_BOT_TOKEN` |
3. Open **Set Channel** and set the policy channel.
4. In **Send Summary**, replace `REPLACE_WITH_SLACK_CHANNEL_ID`.
5. **Activate** the workflow.

## Customizing

- **Email instead of Slack:** swap **Send Summary** for an email node.
- **Multiple channels:** duplicate the chain or loop a channel list.
- **Cadence:** edit **Every Day**.

## Cost

Per matching new video = **0 credits** for the daily check (free) + **1 credit** for the transcript + one LLM call. Quiet days post nothing.

## Notes

- The briefing is instructed to be **factual and neutral — an informational summary, not an endorsement.** Always review before forwarding.
- "New" detection uses workflow static data (last-seen video ID), so it won't re-post the same video.
