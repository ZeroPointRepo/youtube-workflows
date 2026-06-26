# 29 · Student Q&A → Video Timestamp Lookup

Ask a question against a course channel and get a concise answer plus the exact video and timestamps where it's covered. It searches *within* the channel for the most relevant lecture, transcribes it, and an LLM answers using only that transcript — pointing you to the moments to watch.

```
Start → Set Question & Channel → Search Channel → Pick Best Match → Get Transcript (TranscriptAPI)
      → Build Timestamped Text → Answer with Timestamps (LLM) → Format Answer
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Question & Channel** | Edit Fields | The student `question` and the course `channel` (@handle/URL/UC… ID). |
| **Search Channel** | HTTP Request | `GET /api/v2/youtube/channel/search?channel=…&q=…` — searches within the channel. |
| **Pick Best Match** | Code | Takes the most relevant captioned result. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Pulls that video's transcript (*continue on error*). |
| **Build Timestamped Text** | Code | Builds a `[h:mm:ss]`-timestamped transcript. |
| **Answer with Timestamps (LLM)** | HTTP Request | Returns `{ answer, timestamps[], found }` JSON. |
| **Format Answer** | Code | Clean answer + source video + timestamp pointers. |

## Endpoint note

This workflow uses **`/api/v2/youtube/channel/search`** (verified working: `channel` + `q` → captioned, relevance-ranked results within the channel) to locate the right lecture — exactly the intended pattern for "find the relevant video in this course."

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `student-qa-timestamp-lookup.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Question & Channel** and set the `question` and `channel`.
5. Click **Test workflow**. **Format Answer** holds the answer + timestamps.

## Customizing

- **Playlist instead of channel:** swap **Search Channel** for `playlist/videos` + a keyword filter if your course is a playlist.
- **Multiple candidate videos:** raise the count in **Pick Best Match** and answer across the top few.

## Cost

Per question = **1 credit** for the channel search + **1 credit** for the one transcript + one LLM call.

## Notes

- `found` is `false` when the matched video doesn't actually address the question — surface that to the student instead of a guessed answer.
