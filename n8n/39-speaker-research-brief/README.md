# 39 · Speaker Research Brief

Prep for a guest, panelist, or keynote in one run. Give it a speaker's name and it searches their talks, transcribes the top results, and produces a one-page brief — bio summary, recurring themes, notable talks, likely talking points, and timestamped evidence snippets you can quote.

```
Start → Set Speaker → Search Videos (TranscriptAPI) → Extract Captioned Results → Keep Top 4
      → Get Transcript (TranscriptAPI) → Collect Transcripts → Speaker Brief (LLM) → Format Brief
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Speaker** | Edit Fields | The speaker to research (`searchQuery`). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Extract Captioned Results** | Code | Keeps only results with captions; one item per video. |
| **Keep Top 4** | Limit | Caps how many talks to transcribe (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Collect Transcripts** | Code | Builds a `[m:ss]`-timestamped corpus so the brief can cite evidence. |
| **Speaker Brief (LLM)** | HTTP Request | Returns `{ bio_summary, themes, notable_talks, likely_talking_points, evidence_snippets }` JSON. |
| **Format Brief** | Code | Clean fields + the `sources` list. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `speaker-research-brief.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Speaker** and enter the name.
5. Click **Test workflow**. **Format Brief** holds the one-page brief.

## Customizing

- **Sharper search:** try `"<name>" keynote` or `"<name>" interview` in **Set Speaker**.
- **Depth:** raise `maxItems` on **Keep Top 4** (each talk = up to 1 credit).
- **Deep-link evidence:** build `https://youtu.be/<id>?t=<seconds>` from each snippet's timestamp.

## Cost

Per run = **1 credit** for the search + up to **1 credit per transcribed talk** + one LLM call.
