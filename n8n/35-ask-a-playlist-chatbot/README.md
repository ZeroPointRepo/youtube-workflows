# 35 · Ask-a-Playlist Chatbot Builder

Build the knowledge base for a chatbot that answers questions about a YouTube playlist. It transcribes the playlist, chunks the text, and emits a ready-to-use dataset — a chatbot **config** row (with a system prompt) plus one **KB chunk** row per passage — so you can wire it straight into a Q&A / RAG agent.

```
Start → Set Playlist → Get Playlist Videos → Extract Video List → Keep First 3 → Get Transcript (TranscriptAPI) → Build Chatbot KB
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Playlist** | Edit Fields | The playlist URL or ID (`playlistUrl`). |
| **Get Playlist Videos** | HTTP Request | `GET /api/v2/youtube/playlist/videos`. |
| **Extract Video List** | Code | One item per video. |
| **Keep First 3** | Limit | Caps how many videos to ingest (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Build Chatbot KB** | Code | Emits a `chatbot_config` row (system prompt + chunk count) followed by `kb_chunk` rows. |

## Output shape

- Config: `{ type: "chatbot_config", playlist, kb_chunks, system_prompt }`
- KB chunk: `{ type: "kb_chunk", id, text, source_title, source_url, chunk_index }`

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `ask-a-playlist-chatbot.json`.
2. Create one **Header Auth** credential and attach it to **both** TranscriptAPI nodes:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Open **Set Playlist** and paste a playlist URL or ID.
4. Click **Test workflow**. **Build Chatbot KB** outputs your config + KB rows.

## Wiring the chatbot

1. Embed the `kb_chunk` rows (see workflow 31) and store them in a vector DB.
2. On a question, retrieve the top chunks and pass them — plus the `system_prompt` from the config row — to an LLM / n8n **AI Agent** node.
3. The agent answers using only the retrieved chunks and cites the `source_title`.

## Cost

Playlist listing = 1 credit/page; transcripts = up to 1 credit per captioned video (capped by **Keep First 3**). Embedding/LLM costs are provider-side.

## Notes

- Related: workflow 04 (RAG chunks) and 31 (embeddings). This one is framed end-to-end as **chatbot data prep**, emitting the config + KB in one pass.
