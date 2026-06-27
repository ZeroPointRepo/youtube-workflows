# Playlist → Quiz / Flashcard Generator

> Turn a lecture playlist into quizzes and flashcards.

A ready-to-use skill for AI agents (Claude, Codex, OpenClaw, Hermes, and others).
It uses **TranscriptAPI** to pull YouTube transcripts — your agent does the
thinking itself, so all you need is a TranscriptAPI key (no separate AI key).

## What it does
It reads a playlist and generates Anki-style flashcards and multiple-choice questions — one row per card, ready to import.

## How it works
1. You give it a lecture playlist.
2. It transcribes the videos.
3. It writes flashcards and quiz questions.
4. You get one row per card, ready to study.

## The TranscriptAPI calls
```
GET https://transcriptapi.com/api/v2/youtube/transcript?video_url=<YOUTUBE_URL>
Authorization: Bearer <YOUR_TRANSCRIPTAPI_KEY>
```
```
GET https://transcriptapi.com/api/v2/youtube/playlist/videos?playlist=<PLAYLIST>
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
