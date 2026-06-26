# 08 · Video → Transcript → LinkedIn Post

Turn any YouTube video into a ready-to-post LinkedIn update. Paste a URL and get back a scroll-stopping hook, a punchy body, a call to action, and hashtags — assembled into a `fullPost` you can paste straight into LinkedIn.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Write LinkedIn Post (LLM) → Format Post
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The YouTube URL to repurpose. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true` for the title). |
| **Build Transcript Text** | Code | Joins the transcript and carries the title forward. |
| **Write LinkedIn Post (LLM)** | HTTP Request | Asks for a JSON post (`response_format: json_object`). |
| **Format Post** | Code | Parses into `hook`, `body`, `cta`, `hashtags`, and `fullPost`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `video-to-linkedin-post.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Video URL** and paste your URL.
5. Click **Test workflow**. The **Format Post** node holds the post.

## Customizing

- **Voice / length:** edit the system prompt in **Write LinkedIn Post** (e.g. first-person, specific audience, word count).
- **Auto-publish:** add a node after **Format Post** to push `fullPost` to the LinkedIn API or a scheduler (Buffer, Hypefury, etc.).
- **No `response_format` support?** Remove that field — **Format Post** falls back to treating the reply as `fullPost`.

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.
