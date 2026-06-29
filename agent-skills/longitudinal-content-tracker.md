# Longitudinal Content Tracker

> Track how a channel's messaging changes week over week.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
On a weekly schedule it checks a channel's latest uploads via the free feed, transcribes only the new videos it hasn't seen, and writes a snapshot per video — summary, themes, and key messages — to track narrative drift over time.

## How it works
1. You pick a channel to track.
2. Each week it checks the free latest-videos feed.
3. It transcribes only the new, unseen videos.
4. You get a snapshot of summary, themes, and key messages.

## The TranscriptAPI calls
```
GET https://transcriptapi.com/api/v2/youtube/transcript?video_url=<YOUTUBE_URL>
Authorization: Bearer <YOUR_TRANSCRIPTAPI_KEY>
```
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
