# Meeting Prep Research Pack

> Walk into any meeting prepared, from the company's own videos.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Give it a company name. It searches that company's videos, transcribes the most relevant ones, and produces a meeting-prep brief — narrative, recent announcements, pain points they target, likely objections, and discovery questions.

## How it works
1. You enter the company you're meeting.
2. It searches and transcribes that company's videos.
3. It builds a brief — narrative, announcements, and pain points.
4. You get likely objections and discovery questions to ask.

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
