# 26 · Video → Short-Form Script Pack

Turn one long-form video into a pack of 3–5 short-form scripts for Reels, Shorts, and TikTok. Each script comes with a hook, a beat-by-beat outline, a suggested caption, a CTA, and hashtags — emitted as one row per script, ready for a content calendar.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Generate Script Pack (LLM) → Format Scripts
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The long-form YouTube URL to repurpose. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true`). |
| **Build Transcript Text** | Code | Joins the transcript and carries the title forward. |
| **Generate Script Pack (LLM)** | HTTP Request | Returns `{ scripts: [...] }` JSON (`response_format: json_object`). |
| **Format Scripts** | Code | Fans out one row per script (`hook`, `beats`, `caption`, `cta`, `hashtags`, source). |

## Output shape (per row)

`scriptNumber`, `scriptTitle`, `hook`, `beats[]`, `caption`, `cta`, `hashtags[]`, `sourceTitle`, `sourceVideoUrl`.

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `video-short-form-script-pack.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Video URL** and paste your long-form video URL.
5. Click **Test workflow**. **Format Scripts** outputs one row per short.

## Customizing

- **How many / platform:** edit the **Generate Script Pack** prompt (e.g. "5 TikTok scripts", target length, tone).
- **Content calendar:** add a Notion/Airtable/Google Sheets node after **Format Scripts** — each row is one scheduled short.

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.
