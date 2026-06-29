# Ask-a-Playlist Chatbot Builder

> Build the knowledge base for a chatbot that answers questions about a playlist.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Point it at a YouTube playlist. It transcribes every video, splits the text into clean chunks, and emits a ready-to-use dataset — a chatbot config plus one knowledge-base chunk per passage — to wire straight into a Q&A or RAG agent.

## How it works
1. You give it a YouTube playlist.
2. It transcribes each video in the playlist.
3. It splits the text into clean, sized knowledge chunks.
4. You get a chatbot config plus one chunk row per passage.

## The TranscriptAPI calls
```
GET https://transcriptapi.com/api/v2/youtube/transcript?video_url=<YOUTUBE_URL>
Authorization: Bearer <YOUR_TRANSCRIPTAPI_KEY>
```
```
GET https://transcriptapi.com/api/v2/youtube/playlist/videos?playlist=<PLAYLIST>
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
