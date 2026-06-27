# 47 · Lecture Chapter Summary with Timestamps

Turn a long lecture into a navigable study guide. Paste a video URL and it transcribes with timestamps and produces a chapter outline — each chapter has a start time, summary bullets, plus key concepts and review prompts for the whole lecture.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Timestamped Text → Chapter Summary (LLM) → Format Chapters
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The lecture URL. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true`). |
| **Build Timestamped Text** | Code | Builds a `[m:ss]`/`[h:mm:ss]`-timestamped transcript. |
| **Chapter Summary (LLM)** | HTTP Request | Returns `{ chapters, key_concepts, study_prompts, markdown }` JSON. |
| **Format Chapters** | Code | Clean fields + a ready-to-paste `markdown` outline. |

## Output shape

Each chapter: `{ timestamp, title, bullets[] }`. Plus `keyConcepts[]`, `studyPrompts[]`, and a full `markdown` study guide.

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `lecture-chapter-summary.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Video URL** and paste your lecture URL.
5. Click **Test workflow**. **Format Chapters** holds the outline.

## Customizing

- **Chapter count / depth:** nudge the **Chapter Summary** prompt.
- **Clickable chapters:** convert each `timestamp` to `?t=<seconds>` deep links for a YouTube description.
- **Send it somewhere:** add a Notion/Docs node after **Format Chapters** (content only — no auto-publish by default).

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.
