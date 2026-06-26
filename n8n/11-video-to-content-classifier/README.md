# 11 · Video → Transcript → Content Classifier

Auto-tag any YouTube video. Paste a URL and an LLM reads the transcript and returns a structured classification — primary category, categories, content type, topic tags, audience, and a short summary — ready to drop into an Airtable or Notion content database.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Classify (LLM) → Format Classification
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The YouTube URL to classify. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true`). |
| **Build Transcript Text** | Code | Joins the transcript and carries the title forward. |
| **Classify (LLM)** | HTTP Request | Applies a taxonomy and returns JSON (`response_format: json_object`). |
| **Format Classification** | Code | Outputs `primaryCategory`, `categories`, `contentType`, `tags`, `audience`, `summary`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `video-to-content-classifier.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Video URL** and paste your URL.
5. Click **Test workflow**. The **Format Classification** node holds the tags.

## Customizing

- **Your taxonomy:** edit the category and content-type lists in the **Classify (LLM)** system prompt to match your own schema.
- **Send to a database:** add an Airtable / Notion node after **Format Classification** — the fields map cleanly to columns/properties.

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.
