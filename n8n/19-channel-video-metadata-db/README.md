# 19 · Channel → Full Video Metadata DB

Build a database of a channel's uploads. Give it any channel reference, and it resolves the canonical channel ID, lists the channel's videos, and emits one clean row per video (ID, title, link, length, views) — ready to push into Google Sheets, Airtable, Notion, or your own database.

```
Start → Set Channel → Resolve Channel → List Channel Videos → Format Metadata Rows
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Channel** | Edit Fields | The channel — an `@handle`, channel URL, or `UC…` ID (`channel`). |
| **Resolve Channel** | HTTP Request | `GET /api/v2/youtube/channel/resolve` → canonical `channel_id`. |
| **List Channel Videos** | HTTP Request | `GET /api/v2/youtube/channel/videos` — ~100 uploads per page. |
| **Format Metadata Rows** | Code | One row per video: `channelId`, `videoId`, `title`, `videoUrl`, `lengthText`, `viewCountText`, `index`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `channel-video-metadata-db.json`.
2. Create one **Header Auth** credential and attach it to **both** HTTP nodes:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Open **Set Channel** and set `channel` (replace `REPLACE_WITH_CHANNEL_HANDLE_OR_ID`).
4. Click **Test workflow**. **Format Metadata Rows** outputs one row per video.

## Customizing

- **Store it:** add a Google Sheets (append), Airtable, Notion, or database node after **Format Metadata Rows** — each item is one record.
- **All videos:** `channel/videos` is paginated (~100/page). Loop using the `continuation_token` from the response to page through the whole channel.

## Cost

`channel/resolve` + one page of `channel/videos` is very low cost (no transcripts pulled). Each additional `channel/videos` page is 1 credit.

## Notes

- The uploads listing doesn't include exact publish dates; for per-video timestamps use the `channel/latest` feed (see workflow 05/15) or fetch each video's metadata.
