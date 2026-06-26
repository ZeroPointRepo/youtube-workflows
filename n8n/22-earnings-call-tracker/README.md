# 22 · Earnings-Call Tracker → Analyst Summary → Slack

Monitor a company's IR / investor-relations YouTube channel and, when a new earnings call or investor update is posted, transcribe it and post a concise analyst summary (key numbers, highlights, risks, tone) to Slack. For investors, analysts, and IR teams.

```
Every Day → Set Channel → Get Latest Videos (TranscriptAPI) → Detect New Call → Get Transcript (TranscriptAPI)
          → Build Transcript Text → Analyst Summary (LLM) → Send Summary
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every Day** | Schedule Trigger | Checks daily (or click **Test workflow**). |
| **Set Channel** | Edit Fields | The IR/company channel (`channel`). |
| **Get Latest Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/channel/latest` — **free**. |
| **Detect New Call** | Code | Finds the newest video whose title looks like an earnings/investor update and fires once per new one (keyword match + last-seen ID in workflow static data). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches the call transcript (*continue on error*). |
| **Build Transcript Text** | Code | Joins the transcript, carries the title + URL. |
| **Analyst Summary (LLM)** | HTTP Request | Writes a Markdown analyst brief (numbers, highlights, risks, tone). |
| **Send Summary** | HTTP Request | Posts the brief to Slack (`chat.postMessage`). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.
- A **Slack** bot token (`chat:write`) + channel ID (or swap for email).

## Setup

1. **Import** `earnings-call-tracker.json`.
2. Create three **Header Auth** credentials and attach each to its node:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
   | `Slack - Authorization Bearer` | `Authorization` | `Bearer YOUR_SLACK_BOT_TOKEN` |
3. Open **Set Channel** and set the IR channel (`REPLACE_WITH_IR_CHANNEL_HANDLE_OR_ID`).
4. In **Send Summary**, replace `REPLACE_WITH_SLACK_CHANNEL_ID` with your channel ID.
5. **Activate** the workflow.

## Customizing

- **What counts as an earnings video:** edit the `KEYWORDS` list in **Detect New Call** (e.g. add your ticker, "call", "update").
- **Email instead of Slack:** swap **Send Summary** for an email node.
- **Cadence:** earnings are quarterly, so a daily check is plenty; adjust **Every Day** if needed.

## Cost

Per matching new call = **0 credits** for the daily check (free endpoint) + **1 credit** for the transcript + one LLM call. Days with no new earnings video post nothing.

## Notes

- The summary uses only figures stated in the transcript — it's instructed never to invent numbers. Always verify against the official filing before acting on it.
- "New call" detection uses workflow static data (last-seen earnings video ID), so it won't re-post the same call.
