# 49 · Discourse Analysis Pipeline

Analyze how a topic is talked about across YouTube. Give it a topic, and it searches, transcribes the top results, and produces a neutral thematic analysis — recurring frames, stance clusters, representative evidence snippets (with timestamps), and caveats.

```
Start → Set Topic → Search Videos (TranscriptAPI) → Extract Captioned Results → Keep Top 5
      → Get Transcript (TranscriptAPI) → Collect Transcripts → Discourse Analysis (LLM) → Format Report
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Topic** | Edit Fields | The topic / query to analyze (`searchQuery`). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Extract Captioned Results** | Code | Keeps captioned results; one item per video. |
| **Keep Top 5** | Limit | Caps how many videos to analyze (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Collect Transcripts** | Code | Builds a `[m:ss]`-timestamped corpus. |
| **Discourse Analysis (LLM)** | HTTP Request | Returns `{ recurring_frames, stance_clusters, evidence_snippets, caveats }` JSON. |
| **Format Report** | Code | Clean fields + the `sources` list. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `discourse-analysis-pipeline.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Topic** and enter your topic.
5. Click **Test workflow**. **Format Report** holds the analysis.

## Customizing

- **Breadth:** raise `maxItems` on **Keep Top 5** for a wider sample.
- **Angle:** edit the **Discourse Analysis** prompt (e.g. focus on sentiment, rhetoric, or sourcing).

## Cost

Per run = **1 credit** for the search + up to **1 credit per transcribed video** + one LLM call.

## Notes

- **Research aid — needs human review.** This is an automated, descriptive analysis of a *small sample* of public videos; it is not a representative or rigorous study. The prompt is instructed to stay neutral and not take sides — always review the output and widen the sample before drawing conclusions.
