# 27 · Competitor Analysis Agent

Give it a competitor or brand name and it searches YouTube, transcribes the top captioned videos, and produces a competitor-intelligence report — positioning, repeated claims, product themes, common objections, and notable quotes — grounded only in what the videos actually say.

```
Start → Set Competitor → Search Videos (TranscriptAPI) → Extract Captioned Results → Keep Top 5
      → Get Transcript (TranscriptAPI) → Collect Transcripts → Competitor Report (LLM) → Format Report
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Competitor** | Edit Fields | The competitor / brand to research (`searchQuery`). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Extract Captioned Results** | Code | Keeps only results with captions; one item per video. |
| **Keep Top 5** | Limit | Caps how many videos to transcribe (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Collect Transcripts** | Code | Combines into one bounded corpus. |
| **Competitor Report (LLM)** | HTTP Request | Writes the Markdown intel report. |
| **Format Report** | Code | Outputs `report` + the `sources` list. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `competitor-analysis-agent.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and the OpenAI credential to the LLM node.
4. Open **Set Competitor** and enter the brand/competitor.
5. Click **Test workflow**. **Format Report** holds the report.

## Customizing

- **Report sections:** edit the **Competitor Report** system prompt.
- **Depth:** raise `maxItems` on **Keep Top 5** (each video = up to 1 credit).
- **Search angle:** try queries like `"<brand> review"`, `"<brand> vs"`, or `"<brand> alternative"`.

## Cost

Per run = **1 credit** for the search + up to **1 credit per transcribed video** + one LLM call.

## Notes

- Filtering on `hasCaptions` means you don't spend credits on videos that can't be transcribed.
