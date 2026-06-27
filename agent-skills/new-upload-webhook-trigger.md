# New-Upload → Webhook Trigger

> Fire a webhook the moment a channel posts a new video.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
A lightweight trigger: it watches a channel and calls your webhook only when a genuinely new video appears — no duplicates.

## How it works
1. You pick a channel and a webhook URL.
2. It checks the channel on a schedule.
3. When a brand-new video appears, it fires your webhook.
4. It remembers the last video so you never get duplicates.

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
