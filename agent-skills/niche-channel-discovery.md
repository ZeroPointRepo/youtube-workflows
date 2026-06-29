# Niche Channel Discovery Tool

> Find the channels that own a niche, ranked.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Give it a topic. It searches YouTube, de-dupes channels across the results, scores each by frequency, verified status, and reach, and writes a ranked prospect list with sample videos. No AI, no transcripts — fast and cheap.

## How it works
1. You enter a topic or niche.
2. It searches YouTube and collects the channels.
3. It de-dupes and scores each channel.
4. You get a ranked prospect list with sample videos.

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
