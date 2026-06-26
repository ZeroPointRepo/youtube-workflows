# 32 · Channel Sentiment Pipeline

Monitor a channel's latest videos and score each one for sentiment and topics. It pulls the newest uploads, transcribes a small set, runs LLM sentiment scoring + topic tagging per video, and outputs one row per video for Sheets/Airtable/CSV.

```
Start → Set Channel → Get Latest Videos (TranscriptAPI) → Extract Recent Videos → Keep First 3
      → Get Transcript (TranscriptAPI) → Build Sentiment Input → Has Transcript? → Score Sentiment (LLM) → Format Rows
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Channel** | Edit Fields | The channel to analyze (`channel`). |
| **Get Latest Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/channel/latest` — **free**. |
| **Extract Recent Videos** | Code | One item per recent video. |
| **Keep First 3** | Limit | Caps how many to score (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Build Sentiment Input** | Code (per item) | Joins transcript + carries title/URL. |
| **Has Transcript?** | Filter | Drops videos without captions. |
| **Score Sentiment (LLM)** | HTTP Request | Returns `{ sentiment, sentiment_score, topics, summary }` JSON. |
| **Format Rows** | Code (per item) | One sentiment row per video. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `channel-sentiment-pipeline.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Channel** and set the channel.
5. Click **Test workflow**. **Format Rows** outputs one row per video.

## Customizing

- **Schedule it:** swap **Start** for a Schedule Trigger to track sentiment over time.
- **Volume:** raise `maxItems` on **Keep First 3** (each video = up to 1 credit).
- **Destination:** add a Sheets/Airtable node after **Format Rows**.

## Cost

Per run = **0 credits** for the channel check (free) + up to **1 credit per transcribed video** (capped by **Keep First 3**) + one LLM call per video.
