# 14 · Video → Transcript → Podcast Show Notes + Chapters

Generate publish-ready show notes for any video or podcast episode. It transcribes with timestamps and asks an LLM for a summary, chapter markers (starting at 0:00), and full Markdown show notes — plus a ready-to-paste `chapterMarkers` block in YouTube's chapter format.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Write Show Notes (LLM) → Format Show Notes
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The YouTube URL / episode to write up. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true`). |
| **Build Transcript Text** | Code | Builds a `[m:ss]` / `[h:mm:ss]`-timestamped transcript so chapters carry real times. |
| **Write Show Notes (LLM)** | HTTP Request | Returns JSON `{ summary, chapters[], show_notes }` (`response_format: json_object`). |
| **Format Show Notes** | Code | Outputs `summary`, `chapters`, a `chapterMarkers` string, and `showNotes` (Markdown). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `video-to-podcast-show-notes.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Video URL** and paste your URL.
5. Click **Test workflow**. **Format Show Notes** holds the notes; `chapterMarkers` pastes straight into a YouTube description.

## Customizing

- **Chapter count / detail:** nudge the **Write Show Notes** prompt (e.g. "8-12 chapters").
- **Publish:** push `showNotes` to your CMS and `chapterMarkers` into the video description.

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.

## Notes

- The prompt requires the first chapter to start at `0:00` (YouTube's requirement for clickable chapters).
- Chapter times are drawn from transcript timestamps — accurate to the segment.
