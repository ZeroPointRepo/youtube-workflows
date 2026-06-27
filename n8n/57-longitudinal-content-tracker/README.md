# 57 · Longitudinal Content Tracker

Watch how a channel's messaging changes over time. On a weekly schedule it checks a channel's latest uploads (via the **free** `channel/latest` feed), transcribes only the **new** videos it hasn't seen before, and writes a snapshot row per video — summary, themes, and key messages — so you can track narrative drift week over week.

```
Weekly Schedule → Set Channel → Get Latest (channel/latest) → Extract New Videos → Keep First 5
      → Get Transcript (TranscriptAPI) → Build Snapshot Input → Has Transcript? → Summarize (LLM) → Format Snapshot Row
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Weekly Schedule** | Schedule Trigger | Runs weekly (edit the cadence as you like). |
| **Set Channel** | Edit Fields | The channel to track (`channel` — handle or ID). |
| **Get Latest (channel/latest)** | HTTP Request | `GET /api/v2/youtube/channel/latest` — **free**. Returns recent videos with publish dates. |
| **Extract New Videos** | Code | Emits only videos not seen on previous runs (dedupes via workflow static data). |
| **Keep First 5** | Limit | Caps transcripts per run (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Transcribes each new video (*continue on error*). |
| **Build Snapshot Input** | Code (per item) | Carries id/title/published + builds timestamped text. |
| **Has Transcript?** | Filter | Drops videos without captions. |
| **Summarize (LLM)** | HTTP Request | Returns `{ summary, themes, key_messages }`. |
| **Format Snapshot Row** | Code (per item) | One snapshot row: `{ videoId, title, published, videoUrl, summary, themes, keyMessages }`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `longitudinal-content-tracker.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to **Summarize**.
4. Open **Set Channel** and set the channel to track.
5. **Activate** the workflow so the schedule runs (or click **Test workflow** to try it once).

## Customizing

- **Cadence:** edit **Weekly Schedule** (daily, weekly, monthly).
- **Persist snapshots:** add a Sheets / Airtable / DB node after **Format Snapshot Row** to build a time series.
- **Per-run budget:** raise/lower `maxItems` on **Keep First 5**.

## Cost

The **trigger is free** (`channel/latest`). You only spend **1 credit per new video transcribed** + one LLM call each. No new videos = **0 transcript credits**.

## Notes

- **The first run seeds the baseline.** On the very first execution every recent video looks "new," so it will transcribe up to the **Keep First 5** cap and remember those IDs; subsequent runs only process genuinely new uploads (deduped via workflow static data — no database needed).
