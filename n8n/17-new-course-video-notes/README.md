# 17 · New Course Video → Auto Study Notes → Notion

Follow a course or lecture channel and turn every new upload into structured study notes — summary, key concepts, definitions, and study questions — saved to a Notion database. Built for students, edtech, and course operators.

```
Every Day → Set Channel → Get Latest Videos (TranscriptAPI) → Detect New Lecture → Get Transcript (TranscriptAPI)
          → Build Transcript Text → Generate Study Notes (LLM) → Format Notes → Save to Notion
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every Day** | Schedule Trigger | Checks once a day (or click **Test workflow**). |
| **Set Channel** | Edit Fields | The course channel (`@handle`, URL, or `UC…` ID). |
| **Get Latest Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/channel/latest` — **free**. |
| **Detect New Lecture** | Code | Proceeds only on a genuinely new upload (last-seen ID in workflow static data). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches the lecture transcript (*continue on error*). |
| **Build Transcript Text** | Code | Joins the transcript and carries the title + URL forward. |
| **Generate Study Notes (LLM)** | HTTP Request | Returns structured notes JSON (`response_format: json_object`). |
| **Format Notes** | Code | Parses into `summary`, `keyConcepts`, `definitions`, `studyQuestions`, `markdown`. |
| **Save to Notion** | HTTP Request | Creates a page in your Notion database (`POST /v1/pages`). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.
- A **Notion** integration token + a database shared with it.

## Setup

1. **Import** `new-course-video-notes.json`.
2. Create three **Header Auth** credentials and attach each to its node:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
   | `Notion - Authorization Bearer` | `Authorization` | `Bearer YOUR_NOTION_INTEGRATION_TOKEN` |
3. Open **Set Channel** and set the course channel.
4. Prepare a Notion database with properties `Name` (Title), `URL` (URL), `Notes` (Text); share it with your integration and paste its ID into **Save to Notion** (`REPLACE_WITH_NOTION_DATABASE_ID`).
5. **Activate** the workflow.

## Customizing

- **Notes shape:** edit the **Generate Study Notes** prompt (e.g. add "flashcards" or "exam tips").
- **Email / Google Docs instead of Notion:** swap **Save to Notion** for an email or Google Docs node — `markdown` is ready to send.

## Cost

Per new lecture = **1 TranscriptAPI credit** (the daily check is free) + one LLM call.

## Notes

- Notion text properties cap at 2000 characters, so `markdown` is truncated to fit; store the full notes in the page body if you need everything.
- "New" detection uses workflow static data (last-seen video ID), so re-runs don't re-create notes for the same lecture.
