# YouTube Fact-Check Assistant

> Turn a video into a claim-by-claim review sheet for a human checker.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Paste a video URL. It transcribes with timestamps and extracts the distinct checkable claims — stats, dates, events, attributions — one per row, with a timestamp deep-link and a suggested way to verify. It never rates truth; a person does.

## How it works
1. You paste a video URL.
2. It transcribes the video with timestamps.
3. It extracts checkable factual claims, one per row.
4. You get a review sheet to verify each claim yourself.

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
