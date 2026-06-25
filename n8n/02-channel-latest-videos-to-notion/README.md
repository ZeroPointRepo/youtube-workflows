# 02 · Channel → Latest Videos → AI Summaries → Notion

A set-and-forget content monitor. Once a day it reads a YouTube channel's newest videos, fetches a transcript for each, summarizes them with an LLM, and adds a row to a Notion database. No YouTube API key required — it uses the channel's public RSS feed.

```
Every Day → Get Channel Feed (RSS) → Keep Latest 3 → Get Transcript (TranscriptAPI)
          → Prepare Summary Input → Has Transcript? → Summarize (LLM) → Save to Notion
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every Day** | Schedule Trigger | Runs daily at 08:00 (or click **Test workflow** to run now). |
| **Get Channel Feed (RSS)** | RSS Read | Reads `youtube.com/feeds/videos.xml?channel_id=...` — no API key. |
| **Keep Latest 3** | Limit | Caps how many videos to process per run. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript. Set to *continue on error* so videos without captions don't stop the batch. |
| **Prepare Summary Input** | Code | Joins transcript text and carries the title + URL forward. |
| **Has Transcript?** | Filter | Drops videos that returned no transcript. |
| **Summarize (LLM)** | HTTP Request | Summarizes each transcript via an OpenAI-compatible endpoint. |
| **Save to Notion** | HTTP Request | Creates a page in your Notion database (`POST /v1/pages`). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.
- A **Notion** integration token + a database shared with that integration.

## Setup

1. **Import** `channel-latest-videos-to-notion.json`.
2. Create three **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
   | `Notion - Authorization Bearer` | `Authorization` | `Bearer YOUR_NOTION_INTEGRATION_TOKEN` |
3. Attach each credential to its HTTP Request node.
4. **Find your channel ID** (starts with `UC...`): open the channel, View Page Source, search for `"channelId"` — or use any "channel ID finder". Paste it into **Get Channel Feed (RSS)** in place of `REPLACE_WITH_YOUTUBE_CHANNEL_ID`.
5. **Prepare Notion:** create a database with these properties, then share it with your integration:
   - `Name` — **Title**
   - `URL` — **URL**
   - `Summary` — **Text**
   Copy the database ID from its URL and paste it into **Save to Notion** in place of `REPLACE_WITH_YOUR_NOTION_DATABASE_ID`.
6. Click **Test workflow** to run immediately, or let the daily schedule handle it.

## Customizing

- **More/fewer videos:** change `maxItems` on **Keep Latest 3**.
- **Different schedule:** edit **Every Day** (hourly, weekly, etc.).
- **Different destination:** replace **Save to Notion** with a Google Sheets, Airtable, Slack, or webhook node — everything upstream stays the same.

## Cost

Each run = up to **1 TranscriptAPI credit per video** (only for videos that have captions) + your LLM cost per summary. Videos without captions cost **0 credits** and are skipped.

## Notes

- The RSS feed returns roughly the channel's 15 most recent videos; **Keep Latest 3** trims that.
- Notion text properties cap at 2000 characters, so the summary is truncated to fit.
