# 30 · Multi-Channel Content Database Ingester

Point it at a list of channels and it resolves each one, lists their videos, and emits unified rows for a content database — channel metadata, video ID, title, URL, index, length, and view count. Low-credit: no transcripts by default.

```
Start → Set Channel List → Resolve Channel → List Channel Videos → Format Rows
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Channel List** | Code | Your list of channels (edit the array — `@handle`, URL, or `UC…` ID). |
| **Resolve Channel** | HTTP Request | `GET /api/v2/youtube/channel/resolve` → canonical `channel_id` (per channel). |
| **List Channel Videos** | HTTP Request | `GET /api/v2/youtube/channel/videos` (per channel, ~100/page). |
| **Format Rows** | Code | One unified row per video across all channels. |

## Output shape (per row)

`channelId`, `channelTitle`, `channelHandle`, `videoId`, `title`, `videoUrl`, `index`, `lengthText`, `viewCountText`.

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `multi-channel-content-db.json`.
2. Create one **Header Auth** credential and attach it to **both** HTTP nodes:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Edit **Set Channel List** with your channels.
4. Click **Test workflow**. **Format Rows** outputs unified rows across all channels.

## Customizing

- **Store it:** add a Google Sheets (append), Airtable, Notion, or database node after **Format Rows**.
- **Schedule it:** swap **Start** for a Schedule Trigger to ingest on a cadence.
- **All videos:** `channel/videos` is paginated (~100/page); loop on `continuation_token` to page through each channel fully.

## Cost

Low: `channel/resolve` (very low) + one `channel/videos` page (1 credit) **per channel**. **No transcripts** are pulled by default.

## Notes

- The uploads listing doesn't include exact publish dates; use `channel/latest` (workflows 05/15) if you need per-video timestamps.
