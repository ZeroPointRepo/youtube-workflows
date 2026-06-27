# 58 · Course Glossary Builder

Build a glossary for an online course or lecture playlist. Give it a playlist and it transcribes the videos and extracts key terms with **plain-English definitions**, a source video + timestamp deep-link, and optional study notes — a ready-to-study glossary for learners.

```
Start → Set Playlist → Get Playlist Videos → Extract Video List → Keep First 5
      → Get Transcript (TranscriptAPI) → Build Glossary Input → Has Transcript? → Extract Terms (LLM) → Format Glossary
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Playlist** | Edit Fields | The course playlist (`playlistUrl` — URL or ID). |
| **Get Playlist Videos** | HTTP Request | `GET /api/v2/youtube/playlist/videos`. |
| **Extract Video List** | Code | One item per video. |
| **Keep First 5** | Limit | Caps how many to transcribe (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Build Glossary Input** | Code (per item) | Builds a `[m:ss]`-timestamped transcript. |
| **Has Transcript?** | Filter | Drops videos without captions. |
| **Extract Terms (LLM)** | HTTP Request | Returns `terms:[{term, definition, timestamp, study_note}]`. |
| **Format Glossary** | Code (per item) | Flattens to one row per term, each with a `deepLink` to where it's introduced. |

## Output shape

Each row: `{ term, definition, studyNote, timestamp, deepLink, sourceTitle, videoUrl }`.

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `course-glossary-builder.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to **Extract Terms**.
4. Open **Set Playlist** and paste the course playlist URL or ID.
5. Click **Test workflow**. **Format Glossary** holds the glossary terms.

## Customizing

- **Coverage:** raise `maxItems` on **Keep First 5** to cover the whole course.
- **Dedupe terms:** add a step to merge duplicate `term`s across videos.
- **Export:** add a Notion / Docs / Anki node after **Format Glossary** to build a study deck (`deepLink` jumps to where the term is taught).

## Cost

`playlist/videos` 1 credit/page + up to 1 credit per captioned video + one LLM call each.

## Notes

- **Educational aid — verify domain accuracy.** Definitions are AI-generated from the transcript and the prompt is told to define only terms the video actually explains, but always **review terms against an authoritative source** before relying on them for study or teaching.
