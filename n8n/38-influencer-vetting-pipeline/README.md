# 38 · Influencer Vetting Pipeline

Vet a creator before you partner with them. Give it a channel handle and it resolves the channel, samples a few recent videos' transcripts, and scores brand-safety and fit with an LLM — outputting a vetting row with a score, safety flags, and a rationale.

```
Start → Set Channel → Resolve Channel → List Channel Videos → Extract Sample → Keep First 3
      → Get Transcript (TranscriptAPI) → Collect Vetting Input → Score Brand Safety (LLM) → Format Vetting Row
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Channel** | Edit Fields | The creator's channel (`channel` — @handle, URL, or UC… ID). |
| **Resolve Channel** | HTTP Request | `GET /api/v2/youtube/channel/resolve` → canonical `channel_id`. |
| **List Channel Videos** | HTTP Request | `GET /api/v2/youtube/channel/videos`. |
| **Extract Sample** | Code | One item per recent video. |
| **Keep First 3** | Limit | Samples a small N (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches the sampled transcripts (*continue on error*). |
| **Collect Vetting Input** | Code | Combines the sample into one corpus. |
| **Score Brand Safety (LLM)** | HTTP Request | Returns `{ overall_fit_score, brand_safety, flags, rationale, recommended }` JSON. |
| **Format Vetting Row** | Code | One vetting row per creator. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `influencer-vetting-pipeline.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to all three TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Channel** and set the creator's channel.
5. Click **Test workflow**. **Format Vetting Row** outputs the vetting result.

## Customizing

- **Sample size:** raise `maxItems` on **Keep First 3** for a deeper sample (more credits).
- **Criteria:** edit the **Score Brand Safety** prompt (add your brand's red lines, audience fit, niche relevance).
- **Batch vetting:** feed a list of channels in via a preceding loop, one vetting row per creator.

## Cost

Per creator = `channel/resolve` (low) + one `channel/videos` page (1 credit) + up to **1 credit per sampled video** (capped by **Keep First 3**) + one LLM call.

## Notes

- This is a **screening aid**, not a verdict — the LLM scores from a small transcript sample. Always review flagged creators manually before partnering.
