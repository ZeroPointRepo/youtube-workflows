# 28 · YouTube SEO Title & Description Analyzer

Search a keyword, study the top-ranking videos (titles, view counts, lengths, and transcript snippets), and get back SEO patterns plus concrete title/description recommendations — for creators and marketers planning what to publish.

```
Start → Set Keyword → Search Videos (TranscriptAPI) → Extract Captioned Results → Keep Top 6
      → Get Transcript (TranscriptAPI) → Build SEO Corpus → Analyze SEO (LLM) → Format SEO
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Keyword** | Edit Fields | The target keyword (`searchQuery`). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Extract Captioned Results** | Code | Keeps captioned results with title/views/length/published. |
| **Keep Top 6** | Limit | How many top results to analyze (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Pulls each transcript (*continue on error*). |
| **Build SEO Corpus** | Code | Joins each title + view/length/published + a transcript snippet (matched by video ID). |
| **Analyze SEO (LLM)** | HTTP Request | Returns `{ patterns, title_recommendations, description_recommendations, target_keywords }` JSON. |
| **Format SEO** | Code | Clean fields for your planning doc. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `youtube-seo-analyzer.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Keyword** and enter your target keyword.
5. Click **Test workflow**. **Format SEO** holds the analysis + recommendations.

## Customizing

- **Recommendation count/format:** edit the **Analyze SEO** prompt.
- **Breadth:** raise `maxItems` on **Keep Top 6**.

## Cost

Per run = **1 credit** for the search + up to **1 credit per transcribed video** (capped by **Keep Top 6**) + one LLM call.

## Notes

- YouTube descriptions aren't exposed by these endpoints, so the analyzer works from **titles + view/length signals + transcript content** (the strongest available ranking signals). Description recommendations are generated from those patterns.
