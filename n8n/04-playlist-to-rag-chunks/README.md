# 04 · Playlist / Channel → Transcripts → RAG Chunks

Turn a whole playlist (or a channel's uploads) into clean, embedding-ready text chunks for a vector database. Each output item is a `{ id, text, metadata }` object — drop an embeddings node + vector-store node on the end and you've got a YouTube knowledge base for RAG or an MCP/agent tool.

```
Start → Set Playlist Source → Get Playlist Videos (TranscriptAPI) → Extract Video List
      → Keep First 5 → Get Transcript (TranscriptAPI) → Chunk for RAG
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Playlist Source** | Edit Fields | The playlist URL or ID (`PL…`, `UU…`, etc.). A channel's uploads playlist is `UU` + the channel ID minus its `UC` prefix. |
| **Get Playlist Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/playlist/videos` — lists ~100 videos per page. |
| **Extract Video List** | Code | Splits the response into one item per video (id + watch URL). |
| **Keep First 5** | Limit | Caps how many videos to transcribe per run (raise as needed). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript. *Continue on error* so videos without captions don't stop the batch. |
| **Chunk for RAG** | Code | Splits each transcript into ~200-word overlapping chunks with per-chunk metadata, ready to embed. |

## Output shape

Each item out of **Chunk for RAG**:

```json
{
  "id": "VIDEOID-0",
  "text": "… ~200 words of transcript …",
  "metadata": {
    "video_id": "VIDEOID",
    "video_title": "…",
    "video_url": "https://www.youtube.com/watch?v=VIDEOID",
    "author": "…",
    "language": "en",
    "chunk_index": 0,
    "total_chunks": 12,
    "char_count": 1180
  }
}
```

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `playlist-to-rag-chunks.json`.
2. Create one **Header Auth** credential:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Attach it to **both** HTTP Request nodes (playlist + transcript).
4. Open **Set Playlist Source** and paste a playlist URL or ID in place of `REPLACE_WITH_PLAYLIST_URL_OR_ID`.
5. Click **Test workflow**. The **Chunk for RAG** output is your chunk set.

## Customizing

- **Chunk size / overlap:** edit `CHUNK_WORDS` and `OVERLAP_WORDS` in **Chunk for RAG**.
- **More videos:** raise `maxItems` on **Keep First 5** (each video = up to 1 credit).
- **Whole channel instead of a playlist:** point the first HTTP node at `/api/v2/youtube/channel/videos` with a `channel` query param, or use the channel's `UU…` uploads playlist directly.
- **Embed + store:** add an Embeddings node (OpenAI, Cohere, local) then a vector-store node (Pinecone, Qdrant, pgvector, Supabase) after **Chunk for RAG**. Map `text` → content and `metadata` → metadata.

## Cost

- **Playlist listing:** 1 credit per page (~100 videos/page).
- **Transcripts:** up to 1 credit per video that has captions. Videos without captions cost **0 credits** and are skipped.
