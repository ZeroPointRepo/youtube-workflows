# Multi-Video Content Calendar Auditor

> Audit a channel's uploads for themes, gaps, and opportunities.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Give it a channel. It lists the recent videos using cheap metadata only, then AI analyzes the titles, views, and lengths to surface recurring themes, content gaps, and a list of recommended next videos.

## How it works
1. You pick a channel to audit.
2. It lists recent videos using cheap metadata only.
3. AI finds themes, gaps, and opportunities.
4. You get recommended next-video ideas.

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
