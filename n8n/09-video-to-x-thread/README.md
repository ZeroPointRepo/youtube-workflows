# 09 · Video → Transcript → X (Twitter) Thread

Turn any YouTube video into a numbered 10-tweet X thread. Paste a URL and get back a hook tweet, eight idea tweets, and a closing CTA — assembled into a ready-to-post `threadText` plus the raw `tweets` array for posting via an API.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Write X Thread (LLM) → Format Thread
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The YouTube URL to repurpose. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true`). |
| **Build Transcript Text** | Code | Joins the transcript and carries the title forward. |
| **Write X Thread (LLM)** | HTTP Request | Asks for a JSON array of 10 tweets (`response_format: json_object`). |
| **Format Thread** | Code | Numbers the tweets into `threadText` and exposes the `tweets` array + `tweetCount`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `video-to-x-thread.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Video URL** and paste your URL.
5. Click **Test workflow**. The **Format Thread** node holds the thread.

## Customizing

- **Length:** change "10-tweet" in the **Write X Thread** prompt for a shorter/longer thread.
- **Auto-post:** the `tweets` array is ready to loop over and post via the X API (each tweet replying to the previous). Add a Split Out / HTTP loop after **Format Thread**.
- **No `response_format` support?** Remove that field — **Format Thread** falls back gracefully.

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.

## Notes

- The prompt asks the model to keep each tweet under 280 characters, but always sanity-check length before auto-posting — models occasionally run long.
