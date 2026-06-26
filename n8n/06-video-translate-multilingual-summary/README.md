# 06 · Video → Transcript → Translate → Multilingual Summary

Localize any YouTube video. Give it a URL and a target language and it returns a translated title plus a clean, bulleted summary and key topics — all written in your chosen language. Built for global creators and localizers who need the gist of a video in another language fast.

```
Start → Set Inputs → Get Transcript (TranscriptAPI) → Build Transcript Text → Translate & Summarize (LLM) → Format Output
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Inputs** | Edit Fields | The YouTube URL **and** the target language (e.g. `Spanish`, `Japanese`, `Hindi`). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` with `send_metadata=true`. |
| **Build Transcript Text** | Code | Joins segments and carries the title + target language forward. |
| **Translate & Summarize (LLM)** | HTTP Request | Asks an OpenAI-compatible model to translate + summarize into the target language (`response_format: json_object`). |
| **Format Output** | Code | Parses the JSON into `translatedTitle`, `summaryBullets`, `keyTopics`, and a ready-to-paste `summaryText`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `video-translate-multilingual-summary.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Inputs**, paste the YouTube URL, and set `targetLanguage`.
5. Click **Test workflow**. The **Format Output** node holds the multilingual summary.

## Customizing

- **Any language:** just change `targetLanguage` in **Set Inputs** — the prompt is built around it.
- **Full translation vs. summary:** the default produces a condensed summary. For a fuller translation, edit the system prompt in **Translate & Summarize (LLM)** to ask for a complete translated transcript (note: longer output = higher LLM cost).
- **Different model / provider:** change `model` and URL. If your provider doesn't support `response_format`, remove that field — **Format Output** falls back to using the raw reply as `summaryText`.
- **Send it somewhere:** add a node after **Format Output** (Notion, Google Docs, email, a webhook…).

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.

## Notes

- The source transcript can be in any language TranscriptAPI returns; the LLM handles the translation into your target language.
- Very long videos may approach a model's context limit — switch to a larger-context model or trim `transcriptText` in **Build Transcript Text** if needed.
