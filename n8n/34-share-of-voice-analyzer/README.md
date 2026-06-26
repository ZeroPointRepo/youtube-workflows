# 34 · Share-of-Voice Analyzer

Measure how much of a topic's YouTube conversation your brand owns versus competitors. It searches a target keyword and counts how often each brand appears in the top results (title/channel), outputting rank, mention count, share-%, and a top example per brand. Search-metadata only — no transcripts.

```
Start → Set Inputs → Search Videos (TranscriptAPI) → Compute Share of Voice
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Inputs** | Code | The target `keyword` and the `brands` to compare (yours + competitors). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Compute Share of Voice** | Code | One row per brand: `rank`, `mentions`, `sharePct`, `totalResults`, top example. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `share-of-voice-analyzer.json`.
2. Create one **Header Auth** credential and attach it to **Search Videos**:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Edit **Set Inputs**: set the target `keyword` and the `brands` array (your brand + competitors).
4. Click **Test workflow**. **Compute Share of Voice** outputs the ranked rows.

## Customizing

- **Multiple keywords:** duplicate the chain (or loop a keyword list) and aggregate the rows for a fuller picture.
- **Deeper signal:** add `transcript` for the top matches if you need on-screen mentions, not just title/channel — keep N small.
- **Store it:** add a Sheets/Airtable/DB node after **Compute Share of Voice**.

## Cost

**1 credit per keyword searched.** No transcripts by default.

## Notes

- Share is measured against the first page of search results (a consistent, low-cost SoV proxy); widen with multiple keywords for robustness.
