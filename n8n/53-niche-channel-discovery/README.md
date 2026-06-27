# 53 · Niche Channel Discovery Tool

Find the channels that own a niche. Give it a topic and it searches YouTube, **dedupes channels** across the results, scores each by how often it shows up (plus verified status and reach), and writes a ranked prospect list — channel, handle, URL, sample videos, and a score. No LLM, no transcripts: it's a fast, cheap discovery sweep.

```
Start → Set Topic → Search Videos (TranscriptAPI) → Extract Channels → Score & Rank Channels → Format Ranked List
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Topic** | Edit Fields | The topic / niche to explore (`searchQuery`). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Extract Channels** | Code | One item per result, carrying its channel + video metadata. |
| **Score & Rank Channels** | Code | Dedupes by `channelId`; scores `appearances × 10 + verified + reach`; sorts; assigns rank. |
| **Format Ranked List** | Code (per item) | One row per channel: `rank`, `channel`, `channelUrl`, `verified`, `appearances`, `topViews`, `score`, `sampleVideos`. |

## Output shape

Each row: `{ rank, channel, handle, channelUrl, verified, appearances, topViews, score, sampleVideos[], note }`.

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `niche-channel-discovery.json`.
2. Create one **Header Auth** credential and attach it to **Search Videos**:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Open **Set Topic** and enter your niche (e.g. `home espresso reviews`).
4. Click **Test workflow**. **Format Ranked List** holds the ranked channels.

## Customizing

- **Wider net:** run **Search Videos** several times with different queries (synonyms, sub-topics) and merge before **Score & Rank Channels**.
- **Tune the score:** edit the weights in **Score & Rank Channels** (appearances vs. reach vs. verified).
- **Store it:** add a Sheets / Airtable node after **Format Ranked List** to build a prospect database.

## Cost

**1 TranscriptAPI credit per search call.** No transcript or LLM cost.

## Notes

- **Discovery / research only.** This workflow **does not contact anyone** — every row is a prospect to evaluate manually. There is no outreach, email, or send action, by design.
- Ranking is a heuristic from a single search's results (frequency, verified flag, and view-count text). It's a **starting shortlist**, not a definitive authority ranking — widen the queries and review before acting.
