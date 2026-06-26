# 31 · Video-to-Embedding Batch Processor

Turn a batch of videos into vector embeddings, ready for a vector store. It transcribes each video, splits the text into overlapping chunks, generates an embedding per chunk, and emits Supabase-pgvector-ready rows (`id`, `content`, `embedding`, `metadata`).

```
Start → Set Video List → Get Transcript (TranscriptAPI) → Chunk Transcripts → Generate Embedding (OpenAI) → Format pgvector Rows
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video List** | Code | Your batch of video URLs/IDs (keep it small). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Chunk Transcripts** | Code | ~200-word overlapping chunks with metadata. |
| **Generate Embedding (OpenAI)** | HTTP Request | `POST /v1/embeddings` (`text-embedding-3-small`) per chunk. |
| **Format pgvector Rows** | Code | One row per chunk: `{ id, content, embedding, metadata }`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key for embeddings.

## Setup

1. **Import** `video-to-embedding-batch.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach TranscriptAPI to **Get Transcript** and OpenAI to **Generate Embedding**.
4. Edit **Set Video List** with your video URLs/IDs.
5. Click **Test workflow**. **Format pgvector Rows** outputs your embedding rows.

## Customizing

- **Store it:** add a Supabase/Postgres node after **Format pgvector Rows** and insert into a `vector` column + `metadata` jsonb.
- **Chunk size / model:** tune `CHUNK_WORDS`/`OVERLAP` in **Chunk Transcripts** and the `model` in **Generate Embedding**.

## Cost

**1 TranscriptAPI credit per captioned video** + your OpenAI embedding cost per chunk. Keep the input list small while testing.

## Notes

- Defaults are intentionally tiny — this is import-test friendly. Scale the list up once you've wired your vector store.
