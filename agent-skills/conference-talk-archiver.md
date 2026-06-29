# Conference / Talk Archiver

> Build a searchable archive of a conference channel's talks.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
Point it at a conference channel. It lists the videos, transcribes each, and extracts structured rows — talk title, speaker, company, summary, tags, and links — ready for Notion, Airtable, or a spreadsheet.

## How it works
1. You pick a conference or event channel.
2. It lists and transcribes the talks.
3. It extracts title, speaker, company, summary, and tags.
4. You get one archive row per talk.

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
