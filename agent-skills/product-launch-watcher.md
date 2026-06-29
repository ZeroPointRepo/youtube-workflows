# Product Launch Watcher

> Get a Slack alert the moment a competitor posts a launch.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Watch competitor channels. Each day it checks their newest videos, matches titles against launch keywords, de-dupes across runs, and pings Slack with what's new. Uses the free channel feed — no transcripts.

## How it works
1. You list competitor channels and launch keywords.
2. Each day it checks each channel's newest uploads.
3. It flags titles that match a launch or announcement.
4. It posts a Slack alert for anything new.

## The TranscriptAPI call
```
GET https://transcriptapi.com/api/v2/youtube/channel/latest?channel=<CHANNEL>
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
