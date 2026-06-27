# 52 · Multi-Video Content Calendar Auditor

Audit a channel's recent uploads and get back themes, gaps, and content opportunities. Give it a channel and it lists the recent videos (metadata only — **no transcripts, so it stays cheap**), then an LLM analyzes the titles, view counts, and lengths to surface recurring themes, content gaps, and a list of recommended next videos.

```
Start → Set Channel → Resolve Channel → List Channel Videos → Extract Video List → Keep Recent 20
      → Build Audit Input → Audit Calendar (LLM) → Format Audit
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Channel** | Edit Fields | The channel to audit (`channel` — handle or ID). |
| **Resolve Channel** | HTTP Request | `GET /api/v2/youtube/channel/resolve`. |
| **List Channel Videos** | HTTP Request | `GET /api/v2/youtube/channel/videos` (metadata only). |
| **Extract Video List** | Code | One item per video: title, view-count text, length text. |
| **Keep Recent 20** | Limit | How many recent videos to audit. |
| **Build Audit Input** | Code | Aggregates the recent videos into one ordered title list. |
| **Audit Calendar (LLM)** | HTTP Request | Returns `{ themes, cadence_observations, content_gaps, opportunities, recommended_next }`. |
| **Format Audit** | Code | Clean audit report + the `sources` list. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `content-calendar-auditor.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to **Resolve Channel** + **List Channel Videos** and OpenAI to **Audit Calendar**.
4. Open **Set Channel** and set the channel handle or ID (e.g. `@channelname`).
5. Click **Test workflow**. **Format Audit** holds the audit.

## Customizing

- **Depth:** raise `maxItems` on **Keep Recent 20** for a wider audit.
- **Angle:** edit the **Audit Calendar** prompt (e.g. focus on series ideas, SEO gaps, or format mix).
- **Compare channels:** run it on a competitor and diff the themes/gaps.

## Cost

Cheap by design: `channel/videos` is **1 credit per page** + one LLM call. **No transcript credits are spent** — the audit is metadata-only.

## Notes

- **Exact publish dates aren't available** from the metadata endpoint, so "cadence" here means topic/series rotation and ordering (most-recent first), **not** precise day-by-day scheduling. The prompt is told this and instructed not to invent dates. For true date-based cadence, add a source that provides timestamps.
- It analyzes **titles and surface metadata**, not video content — treat the output as directional, and confirm against the actual videos.
