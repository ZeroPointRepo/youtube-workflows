# 43 · Channel → Pinecone RAG Ingestion

Build a searchable knowledge base from a whole channel. It resolves the channel, lists its videos, transcribes them, splits the text into overlapping chunks, embeds each chunk, and upserts the vectors into a Pinecone index — a complete YouTube → vector-DB ingestion pipeline.

```
Start → Set Channel → Resolve Channel → List Channel Videos → Extract Video List → Keep First 3
      → Get Transcript (TranscriptAPI) → Chunk for RAG → Generate Embedding (OpenAI) → Upsert to Pinecone
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Channel** | Edit Fields | The channel to ingest (`channel`). |
| **Resolve Channel** | HTTP Request | `GET /api/v2/youtube/channel/resolve` → canonical `channel_id`. |
| **List Channel Videos** | HTTP Request | `GET /api/v2/youtube/channel/videos`. |
| **Extract Video List** | Code | One item per video. |
| **Keep First 3** | Limit | Caps how many videos to ingest (= credit + embedding budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Chunk for RAG** | Code | ~200-word overlapping chunks with `{id, text, metadata}`. |
| **Generate Embedding (OpenAI)** | HTTP Request | `POST /v1/embeddings` per chunk. |
| **Upsert to Pinecone** | HTTP Request | `POST /vectors/upsert` to your Pinecone index host. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key for embeddings.
- A **Pinecone** index + API key (and the index host).

## Setup

1. **Import** `channel-pinecone-rag-ingestion.json`.
2. Create three **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
   | `Pinecone - Api-Key` | `Api-Key` | `YOUR_PINECONE_API_KEY` |
3. Attach TranscriptAPI to both TranscriptAPI nodes, OpenAI to the embedding node, Pinecone to the upsert node.
4. In **Upsert to Pinecone**, replace `REPLACE_WITH_PINECONE_INDEX_HOST` with your index host (e.g. `my-index-abc123.svc.us-east-1.pinecone.io`).
5. Open **Set Channel** and set the channel.
6. Click **Test workflow**.

## Customizing

- **Chunk size / model:** tune `CHUNK_WORDS`/`OVERLAP` in **Chunk for RAG** and the embedding `model`.
- **Volume:** raise `maxItems` on **Keep First 3**. **Batch the upsert** (Pinecone accepts many vectors per call) for large channels.
- **Different vector DB:** swap **Upsert to Pinecone** for Qdrant/Supabase/pgvector — the `{id, text, metadata}` + embedding is portable.

## Cost

`channel/videos` 1 credit/page + up to 1 credit per captioned video + your OpenAI embedding cost. Pinecone per its own pricing.

## Notes

- **No real upsert happens until you configure credentials** — the Pinecone host and key are placeholders, so importing/testing the graph has no side effects. Embedding dimensions must match your Pinecone index.
