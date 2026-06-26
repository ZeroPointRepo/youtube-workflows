# 03 · Single Video → Transcript → SEO Blog-Post Draft

Repurpose any YouTube video into a publish-ready blog post. Paste one URL and get back a structured draft: SEO title, meta description, slug, tags, and a full Markdown article. Built for content marketers turning video into written content.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Generate Blog Draft (LLM) → Format Blog Draft
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The YouTube URL to repurpose (edit this). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` with `send_metadata=true` so the draft can use the real video title. |
| **Build Transcript Text** | Code | Joins transcript segments and carries title/author/thumbnail forward. |
| **Generate Blog Draft (LLM)** | HTTP Request | Asks an OpenAI-compatible model for a JSON blog draft (`response_format: json_object`). |
| **Format Blog Draft** | Code | Parses the JSON into `seoTitle`, `metaDescription`, `slug`, `tags`, and `markdown`. |

## Prerequisites

- An [n8n](https://n8n.io) instance (cloud or self-hosted).
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key for the drafting step.

## Setup

1. **Import** `video-to-seo-blog-post.json` (Workflows → Import from File).
2. Create two **Header Auth** credentials in n8n:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node (imports leave them as `REPLACE_..._CRED_ID`).
4. Open **Set Video URL** and paste the YouTube URL you want.
5. Click **Test workflow**. The **Format Blog Draft** node's output holds the finished draft.

## Customizing

- **Tone / structure:** edit the system prompt in **Generate Blog Draft (LLM)** (e.g. word count, audience, brand voice).
- **Different model / provider:** change the `model` and URL — any OpenAI-compatible endpoint works. If your provider doesn't support `response_format`, remove that field and the **Format Blog Draft** node will fall back to treating the whole reply as the Markdown body.
- **Publish it:** add a node after **Format Blog Draft** to push `markdown` to WordPress, Ghost, a Git commit, or your CMS.

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call. Videos without captions return non-200 and cost **0 credits**.

## Notes

- Very long videos produce long transcripts. `gpt-4o-mini` handles large contexts comfortably, but if you hit a model's context limit, trim `transcriptText` in **Build Transcript Text** or switch to a larger-context model.
