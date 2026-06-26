# 10 · Brand-Mention Monitor → Slack Digest

Track a brand, product, or keyword across YouTube. Once a day it searches for your term, keeps the **new** results (published recently), transcribes them, and posts a Slack digest summarizing how each video mentions your keyword. Where workflow 05 watches a single channel, this one watches **search results** — for marketers and competitive intel.

```
Every Day → Set Keyword → Search Mentions (TranscriptAPI) → Filter New Mentions → Keep Top 5
          → Get Transcript (TranscriptAPI) → Collect Mentions → Summarize Mentions (LLM) → Send Slack Digest
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every Day** | Schedule Trigger | Runs daily at 09:00 (or click **Test workflow**). |
| **Set Keyword** | Edit Fields | The brand/keyword to track (`searchQuery`). |
| **Search Mentions (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Filter New Mentions** | Code | Keeps captioned results published within the lookback window — a stateless "new" gate, no database. |
| **Keep Top 5** | Limit | Caps how many mentions to transcribe per run. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Collect Mentions** | Code | Combines the transcripts into one digest input. |
| **Summarize Mentions (LLM)** | HTTP Request | Writes one bullet per video describing the mention. |
| **Send Slack Digest** | HTTP Request | Posts the digest to Slack via `chat.postMessage`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.
- A **Slack** bot token (`xoxb-…`) with `chat:write`, and the target channel's ID.

## Setup

1. **Import** `brand-mention-monitor-slack.json`.
2. Create three **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
   | `Slack - Authorization Bearer` | `Authorization` | `Bearer YOUR_SLACK_BOT_TOKEN` |
3. Attach the TranscriptAPI credential to **both** HTTP nodes (search + transcript), OpenAI to the LLM node, Slack to the digest node.
4. Open **Set Keyword** and replace `REPLACE_WITH_BRAND_OR_KEYWORD`.
5. In **Send Slack Digest**, replace `REPLACE_WITH_SLACK_CHANNEL_ID` with your channel ID (e.g. `C0123456789`).
6. **Activate** the workflow for the daily run, or click **Test workflow** to try it now.

## Customizing

- **Cadence + window:** edit **Every Day**, and set `LOOKBACK_DAYS` in **Filter New Mentions** to match your interval (default 1 day for a daily run).
- **Volume:** raise `maxItems` on **Keep Top 5** for more mentions per run.
- **Email instead of Slack:** swap **Send Slack Digest** for an email node.

## Cost

Per run = **1 credit** for the search + up to **1 credit per new transcribed mention**. Days with no new captioned mentions cost just the 1 search credit and post nothing.

## Notes

- **De-dupe is stateless.** YouTube search returns a *relative* publish time ("2 days ago"), so the "new" gate approximates age from that text rather than storing history. For exact, no-overlap de-duplication across runs, add n8n's **Remove Duplicates** node keyed on `videoId` (it persists across executions).
- Search is **relevance-ranked**, so very new mentions of a low-volume keyword may not surface immediately; widen the lookback or paginate for fuller coverage.
