# 21 · Playlist → Quiz / Flashcard Generator

Turn a lecture playlist into study material. It transcribes the videos, then an LLM generates Anki-style flashcards (front/back) and multiple-choice quiz questions — emitted as **one row per card**, ready for Notion, Airtable, Anki, or CSV.

```
Start → Set Playlist → Get Playlist Videos → Extract Video List → Keep First 3 → Get Transcript (TranscriptAPI)
      → Collect Transcripts → Generate Quiz & Flashcards (LLM) → Format Cards
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Playlist** | Edit Fields | The lecture playlist URL or ID (`playlistUrl`). |
| **Get Playlist Videos** | HTTP Request | `GET /api/v2/youtube/playlist/videos`. |
| **Extract Video List** | Code | One item per video. |
| **Keep First 3** | Limit | Caps how many lectures to process. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Collect Transcripts** | Code | Combines transcripts into one source-tagged corpus. |
| **Generate Quiz & Flashcards (LLM)** | HTTP Request | Returns `{ flashcards[], quiz[] }` JSON (`response_format: json_object`). |
| **Format Cards** | Code | Fans out into one row per flashcard / quiz item, tagged by `source`. |

## Output shape

- Flashcard row: `{ type: "flashcard", front, back, source }`
- Quiz row: `{ type: "quiz", question, options[], answer, source }`

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `playlist-quiz-flashcards.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and the OpenAI credential to the LLM node.
4. Open **Set Playlist** and paste a playlist URL or ID.
5. Click **Test workflow**. **Format Cards** outputs your study rows.

## Customizing

- **Counts / style:** edit the **Generate Quiz & Flashcards** prompt (more cards, harder quiz, cloze deletions, etc.).
- **Anki:** filter `type === "flashcard"` and map `front`/`back` to an Anki CSV; quiz rows export to Notion/Airtable.
- **Volume:** raise `maxItems` on **Keep First 3** (each lecture = up to 1 credit).

## Cost

Playlist listing = 1 credit/page; transcripts = up to 1 credit per captioned video (capped by **Keep First 3**) + one LLM call.
