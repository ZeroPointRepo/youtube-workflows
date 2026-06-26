# 13 · Video → Transcript → Key Quotes (with Timestamps)

Pull the most tweetable moments out of a video. This workflow transcribes with timestamps and asks an LLM for 5–10 short, near-verbatim quotes, each tagged with the time it was said. It outputs **one item per quote**, so it drops straight into Airtable (or Sheets/Notion) as rows.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Extract Quotes (LLM) → Format Quotes
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The YouTube URL to mine. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true`). |
| **Build Transcript Text** | Code | Builds a `[m:ss]`-timestamped transcript so quotes can carry times. |
| **Extract Quotes (LLM)** | HTTP Request | Returns JSON `{ quotes: [{ quote, timestamp }] }` (`response_format: json_object`). |
| **Format Quotes** | Code | Emits **one item per quote** (`quote`, `timestamp`, `sourceTitle`, `sourceVideoId`). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `key-quotes-extractor.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Video URL** and paste your URL.
5. Click **Test workflow**. **Format Quotes** outputs one row per quote.

## Customizing

- **How many quotes:** change "5-10" in the **Extract Quotes** prompt.
- **Send to Airtable:** add an Airtable node after **Format Quotes** — each item is one record (`quote`, `timestamp`, `sourceTitle`, `sourceVideoId`).
- **Deep-link the timestamp:** build `https://youtu.be/<sourceVideoId>?t=<seconds>` in a follow-up node to jump straight to each quote.

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.

## Notes

- Timestamps are the transcript markers nearest each quote — accurate to the segment, not the exact word.
