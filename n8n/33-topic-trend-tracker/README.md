# 33 · Topic Trend Tracker

Track a topic's momentum on YouTube over time. Once a week it searches a keyword and appends a snapshot — result count plus the top videos' metadata (title, channel, views, length, published) — so you can chart how a topic trends. No transcripts, low-credit.

```
Every Week → Set Keyword → Search Videos (TranscriptAPI) → Build Trend Snapshot
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every Week** | Schedule Trigger | Runs weekly (every 7 days at 08:00). |
| **Set Keyword** | Edit Fields | The topic keyword to track (`searchQuery`). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Build Trend Snapshot** | Code | One snapshot row: `keyword`, `snapshotAt`, `resultCount`, `hasMore`, `topVideos[]`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `topic-trend-tracker.json`.
2. Create one **Header Auth** credential and attach it to **Search Videos**:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Open **Set Keyword** and set your topic.
4. **Activate** the workflow (or click **Test workflow** for one snapshot now).

## Customizing

- **Append to a sheet/DB:** add a Google Sheets (append) / database node after **Build Trend Snapshot** — one row per run builds your trend history.
- **Cadence:** edit **Every Week** (daily, monthly…).
- **More keywords:** duplicate the chain or feed multiple keywords through it.

## Cost

**1 credit per run** (one search). No transcripts are pulled.

## Notes

- `resultCount` reflects the first page of results (a consistent week-over-week proxy for topic volume), and `topVideos` captures the current leaders.
