# Onboarding Video → Internal Wiki Page

> Turn an onboarding video into a clean internal wiki page.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Paste a training video URL. It transcribes the video and produces a Confluence or Notion-style page — summary, step-by-step instructions, resources mentioned, and an FAQ — ready to paste into your knowledge base.

## How it works
1. You paste an onboarding or training video URL.
2. It transcribes the video.
3. It writes a wiki page — summary, steps, resources, and FAQ.
4. You paste it into Confluence, Notion, or your wiki.

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
