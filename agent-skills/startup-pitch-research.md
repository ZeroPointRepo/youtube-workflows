# Startup Pitch Research Tool

> Diligence a startup from what its founders say on camera.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Give it a startup or founder name. It finds their talks and investor-event appearances, transcribes the top results, and produces a research report — claims, traction signals, risks, and open diligence questions.

## How it works
1. You enter a startup or founder name.
2. It searches and transcribes their talks.
3. It extracts claims, traction signals, and risks.
4. You get a research report with open diligence questions.

## The TranscriptAPI calls
```
GET https://transcriptapi.com/api/v2/youtube/transcript?video_url=<YOUTUBE_URL>
Authorization: Bearer <YOUR_TRANSCRIPTAPI_KEY>
```
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
