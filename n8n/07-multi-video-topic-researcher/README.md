# 07 · Multi-Video Topic Researcher → Research Brief

Research any topic across YouTube in one run. Give it a search query and it finds the top videos, transcribes the ones with captions, and asks an LLM to synthesize a structured **research brief** — executive summary, key themes, points of consensus and disagreement, and notable takeaways — grounded only in what the videos actually say.

```
Start → Set Query → Search Videos (TranscriptAPI) → Extract Captioned Results → Keep Top 5
      → Get Transcript (TranscriptAPI) → Collect Transcripts → Synthesize Research Brief (LLM) → Format Brief
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Query** | Edit Fields | The topic to research (edit `searchQuery`). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video` — relevance-ranked results. |
| **Extract Captioned Results** | Code | Keeps only results with captions (`hasCaptions`) and emits one item per video. |
| **Keep Top 5** | Limit | Caps how many videos to transcribe (= credit budget per run). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript. *Continue on error* so a bad one doesn't stop the batch. |
| **Collect Transcripts** | Code | Combines the transcripts into one bounded corpus (capped per source). |
| **Synthesize Research Brief (LLM)** | HTTP Request | Writes the Markdown brief from the corpus. |
| **Format Brief** | Code | Outputs `brief` (Markdown) + the `sources` list. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `multi-video-topic-researcher.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to **both** HTTP nodes (search + transcript) and the OpenAI credential to the LLM node.
4. Open **Set Query** and enter your topic.
5. Click **Test workflow**. The **Format Brief** node holds the finished brief.

## Customizing

- **Breadth:** raise `maxItems` on **Keep Top 5** for more sources (more credits).
- **Source length:** tune `MAX_CHARS_PER_SOURCE` in **Collect Transcripts** to control prompt size.
- **Brief shape:** edit the section list in the **Synthesize Research Brief** system prompt.

## Cost

Per run = **1 credit** for the search + up to **1 credit per transcribed video** (only those with captions) + one LLM call.

## Notes

- Search is **relevance-ranked**, not date-ranked — this brief is about a topic, not the newest uploads (see workflow 10 for monitoring new results over time).
- Filtering on `hasCaptions` up front means you don't spend credits on videos that can't be transcribed.
