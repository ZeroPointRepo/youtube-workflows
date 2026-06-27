# 45 · Customer Testimonial Mining

Find ready-to-use customer quotes across a channel's videos. It transcribes recent videos with timestamps and uses an LLM to surface positive customer testimonials — each with a timestamp, the product/theme it praises, a confidence level, and a suggested marketing use.

```
Start → Set Channel → Resolve Channel → List Channel Videos → Extract Video List → Keep First 3
      → Get Transcript (TranscriptAPI) → Build Timestamped Text → Has Transcript? → Mine Testimonials (LLM) → Format Testimonials
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Channel** | Edit Fields | The channel to mine (`channel`). |
| **Resolve Channel** | HTTP Request | `GET /api/v2/youtube/channel/resolve`. |
| **List Channel Videos** | HTTP Request | `GET /api/v2/youtube/channel/videos`. |
| **Extract Video List** | Code | One item per video. |
| **Keep First 3** | Limit | Caps how many videos to mine (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Build Timestamped Text** | Code (per item) | Builds a `[m:ss]`-timestamped transcript. |
| **Has Transcript?** | Filter | Drops videos without captions. |
| **Mine Testimonials (LLM)** | HTTP Request | Returns `{ testimonials: [...] }` JSON. |
| **Format Testimonials** | Code | One row per testimonial (quote, timestamp, product/theme, confidence, suggested use, source). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `customer-testimonial-mining.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Channel** and set the channel (e.g. your own, or a partner's, with customer interviews/reviews).
5. Click **Test workflow**. **Format Testimonials** outputs one row per quote.

## Customizing

- **Deep-link the quote:** build `https://youtu.be/<id>?t=<seconds>` from each timestamp.
- **Volume:** raise `maxItems` on **Keep First 3**.
- **Store it:** add an Airtable/Notion node after **Format Testimonials**.

## Cost

`channel/videos` 1 credit/page + up to 1 credit per captioned video + one LLM call per video.

## Notes

- **Human-review before public use.** These are AI-extracted quotes with a `confidence` score and a suggested use — always confirm wording, get the customer's permission, and verify context before publishing a testimonial.
