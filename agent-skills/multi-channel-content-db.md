# Multi-Channel Content Database Ingester

> Build one content database from many channels.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Point it at a list of channels. It resolves each one, lists their videos, and returns unified rows for a content database — no transcripts needed.

## How it works
1. You give it a list of channels.
2. It resolves each channel.
3. It lists every channel’s videos.
4. You get unified rows (channel, video, views, length) for your database.

## The TranscriptAPI call


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
