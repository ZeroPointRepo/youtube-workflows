# 20 · Playlist → Full Text Corpus (CSV / JSON)

Turn a whole playlist into a structured research corpus: one row per video with metadata plus the full transcript text. Export to CSV or JSON and you've got a dataset for analysis, search, or fine-tuning.

```
Start → Set Playlist → Get Playlist Videos → Extract Video List → Keep First 10 → Get Transcript (TranscriptAPI) → Format Corpus Rows
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Playlist** | Edit Fields | The playlist URL or ID (`playlistUrl`). |
| **Get Playlist Videos** | HTTP Request | `GET /api/v2/youtube/playlist/videos` (~100/page). |
| **Extract Video List** | Code | One item per video (watch URL + title). |
| **Keep First 10** | Limit | Caps how many videos to transcribe (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Format Corpus Rows** | Code | One row per video: `videoId`, `title`, `author`, `videoUrl`, `language`, `segmentCount`, `wordCount`, `transcriptText`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `playlist-full-text-corpus.json`.
2. Create one **Header Auth** credential and attach it to **both** HTTP nodes:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Open **Set Playlist** and paste a playlist URL or ID.
4. Click **Test workflow**. **Format Corpus Rows** is your corpus.

## Customizing

- **Export:** add a **Convert to File** (CSV) node, a Google Sheets append, or a "Write file" node after **Format Corpus Rows**.
- **Size:** raise `maxItems` on **Keep First 10** (each video = up to 1 credit).
- **Chunk it instead:** for embeddings/RAG, see workflow 04, which emits overlapping chunks rather than one row per video.

## Cost

Playlist listing = 1 credit/page; transcripts = up to 1 credit per captioned video (capped by **Keep First 10**). Videos without captions cost 0 and are skipped.
