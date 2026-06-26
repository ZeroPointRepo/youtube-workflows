# 12 · Video → Transcript → Newsletter Section

Turn a YouTube video into a polished newsletter section, ready for Substack or Beehiiv. Paste a URL and get back a headline, 3–5 crisp insights, a one-line takeaway, and a `markdown` block you can paste straight into your editor.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Write Newsletter Section (LLM) → Format Section
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The YouTube URL to write up. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true`). |
| **Build Transcript Text** | Code | Joins the transcript and carries the title forward. |
| **Write Newsletter Section (LLM)** | HTTP Request | Returns JSON (`response_format: json_object`). |
| **Format Section** | Code | Outputs `headline`, `insights`, `takeaway`, and `markdown`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `video-to-newsletter-section.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Video URL** and paste your URL.
5. Click **Test workflow**. The **Format Section** node holds the section (`markdown` is ready to paste).

## Customizing

- **Voice / length:** edit the **Write Newsletter Section** system prompt for your newsletter's tone and section length.
- **Auto-draft:** add a node after **Format Section** to push `markdown` to your newsletter platform's API or a Google Doc.
- **No `response_format` support?** Remove that field — **Format Section** falls back to using the reply as `markdown`.

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.
