# 42 · Onboarding Video → Internal Wiki Page

Turn an onboarding or training video into a clean internal wiki page. Paste a URL and it transcribes the video and produces a Confluence/Notion-style page — summary, step-by-step instructions, resources/links mentioned, and an FAQ — ready to paste into your knowledge base.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Generate Wiki Page (LLM) → Format Wiki
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The onboarding/training video URL. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true`). |
| **Build Transcript Text** | Code | Joins the transcript and carries the title. |
| **Generate Wiki Page (LLM)** | HTTP Request | Returns `{ title, summary, steps, links, faq, markdown }` JSON. |
| **Format Wiki** | Code | Clean fields + a ready-to-paste `markdown` page. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `onboarding-video-wiki.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Video URL** and paste the video URL.
5. Click **Test workflow**. **Format Wiki** holds the page (`markdown` is ready to paste).

## Customizing

- **Publish it (your call):** add a Confluence / Notion node after **Format Wiki** to create a page — by default this workflow only **produces** the content (no auto-publish), so review before pushing it live.
- **Structure:** edit the **Generate Wiki Page** prompt (add "Prerequisites", "Owners", a "Last reviewed" line, etc.).

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.

## Notes

- No real publish/send happens by default — the workflow outputs the wiki content for a human to review and place. Wire your CMS node when you're ready.
