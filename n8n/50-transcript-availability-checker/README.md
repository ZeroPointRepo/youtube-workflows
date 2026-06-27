# 50 · Transcript Availability Checker

Before a big transcript job, check which videos actually have captions. Feed it a list of video URLs/IDs and it returns a status row per video — available or unavailable, the error reason, a retry recommendation, and a dedupe key. Credit-safe: only successful transcripts cost a credit.

```
Start → Set Video List → Get Transcript (TranscriptAPI) → Classify Availability
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video List** | Code | Your sample of video URLs/IDs to check. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Checks each video. **Batched** (5 at a time, 1s apart), **no auto-retry** (so known-permanent failures aren't re-run), *continue on error*. |
| **Classify Availability** | Code | One row per video: `status` (available/unavailable), `errorReason`, `httpCode`, `retryRecommendation`, `dedupeKey`. |

## Status rows

- Available → `{ status: "available", retryRecommendation: "none" }`
- No captions → `{ errorReason: "no_captions", retryRecommendation: "no" }`
- Invalid ID → `{ errorReason: "invalid_id", retryRecommendation: "no" }`
- Rate-limited / no credits → `{ errorReason: "rate_limited_or_no_credits", retryRecommendation: "later" }`
- Server error → `{ errorReason: "server_error", retryRecommendation: "later" }`

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `transcript-availability-checker.json`.
2. Create one **Header Auth** credential and attach it to **Get Transcript**:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Edit **Set Video List** with your video URLs/IDs.
4. Click **Test workflow**. **Classify Availability** outputs one status row per video.

## Customizing

- **Throughput:** tune `batchSize` / `batchInterval` in **Get Transcript** → Options → Batching.
- **Feed it dynamically:** replace **Set Video List** with a Read-CSV / Sheets / DB source.
- **Skip known failures:** use the `dedupeKey` + `retryRecommendation` to filter out `retry: "no"` videos on subsequent runs.

## Cost

**1 credit per video that has a transcript.** No captions, invalid ID, rate-limited, and server errors all cost **0 credits** — and auto-retry is off, so you never re-spend on a known-permanent failure.

## Notes

- **Run on a small sample first.** The default list is tiny on purpose — confirm behavior, then scale up your input list.
