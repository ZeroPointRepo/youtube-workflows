# Channel → Pinecone RAG Ingestion

> Ingest a whole channel into a Pinecone vector database.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Give it a channel. It lists the videos, transcribes them, splits the text into overlapping chunks, embeds each chunk, and upserts the vectors into Pinecone — a complete YouTube-to-vector-DB pipeline.

## How it works
1. You give it a channel and your Pinecone index.
2. It lists and transcribes the channel's videos.
3. It chunks and embeds the text.
4. It upserts the vectors into your Pinecone index.

## The TranscriptAPI call
```
GET https://transcriptapi.com/api/v2/youtube/transcript?video_url=<YOUTUBE_URL>
Authorization: Bearer <YOUR_TRANSCRIPTAPI_KEY>
```

Transcript response shape:
```json
{ "video_id": "...", "language": "en", "transcript": [ { "text": "...", "start": 0.0, "duration": 1.95 } ] }
```

## What you need
- A **TranscriptAPI key** — free tier includes 100 credits, no credit card. Get one: https://transcriptapi.com/signup
- Nothing else: your agent does the reasoning (summarizing, extracting, classifying) natively — no extra AI API key required.

## Notes
- 1 TranscriptAPI credit per successful transcript.
- Free and open source (MIT). Part of the YouTube workflow library: https://github.com/ZeroPointRepo/youtube-workflows
