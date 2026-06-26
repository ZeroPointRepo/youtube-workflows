# 23 · Playlist → Study Notes Generator

Turn a course or lecture playlist into structured study notes — one set per video. Each video becomes a row with a summary, key concepts, definitions, and section timestamps, ready for Markdown, Notion, Airtable, or CSV.

```
Start → Set Playlist → Get Playlist Videos → Extract Video List → Keep First 3 → Get Transcript (TranscriptAPI)
      → Build Notes Input → Has Transcript? → Generate Study Notes (LLM) → Format Notes
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Playlist** | Edit Fields | The playlist URL or ID (`playlistUrl`). |
| **Get Playlist Videos** | HTTP Request | `GET /api/v2/youtube/playlist/videos`. |
| **Extract Video List** | Code | One item per video. |
| **Keep First 3** | Limit | Caps how many lectures to process (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Build Notes Input** | Code (per item) | Builds a `[h:mm:ss]`-timestamped transcript + title/URL. |
| **Has Transcript?** | Filter | Drops videos without captions. |
| **Generate Study Notes (LLM)** | HTTP Request | Returns `{ summary, key_concepts, definitions, timestamps, markdown }` JSON. |
| **Format Notes** | Code (per item) | One study-notes row per video. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `playlist-study-notes.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and the OpenAI credential to the LLM node.
4. Open **Set Playlist** and paste a playlist URL or ID.
5. Click **Test workflow**. **Format Notes** outputs one notes row per video.

## Customizing

- **Notes depth:** edit the **Generate Study Notes** prompt (add flashcards, exam tips, formulas, etc.).
- **Volume:** raise `maxItems` on **Keep First 3** (each video = up to 1 credit).
- **Destination:** add a Notion/Airtable/Google Sheets node, or a Convert-to-File (CSV) node, after **Format Notes**.

## Cost

Playlist listing = 1 credit/page; transcripts = up to 1 credit per captioned video (capped by **Keep First 3**) + one LLM call per video.

## Notes

- Want quiz questions / flashcards instead of notes? See workflow 21. Want a single combined corpus? See workflow 20.
