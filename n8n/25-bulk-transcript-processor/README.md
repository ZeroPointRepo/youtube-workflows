# 25 · Bulk Transcript Processor (with Rate-Limit Handling)

Process a batch of video URLs/IDs reliably. It fetches transcripts with controlled batching, automatic retry/backoff on transient errors, and per-video success/failure classification — so a few bad IDs or a rate-limit blip never sink the whole run. Credit-safe: failed, 4xx, 5xx, and 429 responses cost **0 credits**.

```
Start → Set Video List → Get Transcript (TranscriptAPI) → Classify Results
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video List** | Code | Your batch of video URLs/IDs (edit the array). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript. **Batching** (5 at a time, 1s apart), **retry on fail** (3 tries, 2s backoff), and *continue on error* so failures don't stop the batch. |
| **Classify Results** | Code | Emits one row per video: `success` (with segment count) or `failed` (with an `errorType`: `no_captions` / `invalid_id` / `rate_limited_or_no_credits` / `server_error`). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `bulk-transcript-processor.json`.
2. Create one **Header Auth** credential and attach it to **Get Transcript**:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Edit **Set Video List** with your video URLs/IDs (replace the `REPLACE_WITH_VIDEO_URL_OR_ID_*` entries).
4. Click **Test workflow**. **Classify Results** outputs one row per video.

## Customizing

- **Throughput vs. rate limits:** tune `batchSize` / `batchInterval` in **Get Transcript** → Options → Batching, and `maxTries` / `waitBetweenTries` (retry) on the same node.
- **Feed it dynamically:** replace **Set Video List** with a Read-CSV / Google Sheets / webhook source that supplies `videoUrl` per item.
- **Route failures:** send `status === "failed"` rows to a log/alert; keep `success` rows for downstream processing.

## Cost

**1 credit per successful transcript only.** Failures (no captions, bad ID, rate-limited, server error) cost **0 credits** — so retries and bad inputs are safe.

## Notes

- Built-in n8n batching + retry/backoff is what makes this rate-limit-friendly; for very large lists, lower the batch size and increase the interval.
