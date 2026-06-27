# Share-of-Voice Analyzer

> Measure how much of a topic your brand owns vs rivals.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Search a topic and it counts how often each brand shows up in the top results — giving you each brand’s share of the conversation.

## How it works
1. You enter a topic and the brands to compare.
2. It searches YouTube for that topic.
3. It counts how often each brand appears in the top results.
4. You get each brand’s share of voice.

## The TranscriptAPI call
```
GET https://transcriptapi.com/api/v2/youtube/search?q=<QUERY>
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
