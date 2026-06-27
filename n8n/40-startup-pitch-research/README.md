# 40 · Startup Pitch Research Tool

Diligence a startup from what its founders say on camera. Give it a startup or founder name and it searches their talks and investor-event appearances, transcribes the top results, and produces a structured research report — claims, traction signals, risks, and open diligence questions.

```
Start → Set Startup → Search Videos (TranscriptAPI) → Extract Captioned Results → Keep Top 4
      → Get Transcript (TranscriptAPI) → Collect Transcripts → Pitch Research (LLM) → Format Report
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Startup** | Edit Fields | The startup / founder to research (`searchQuery`). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Extract Captioned Results** | Code | Keeps captioned results; one item per video. |
| **Keep Top 4** | Limit | Caps how many talks to transcribe (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Collect Transcripts** | Code | Combines into one corpus. |
| **Pitch Research (LLM)** | HTTP Request | Returns `{ summary, claims, traction_signals, risks, open_diligence_questions }` JSON. |
| **Format Report** | Code | Clean report fields + sources. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `startup-pitch-research.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Startup** and enter the name.
5. Click **Test workflow**. **Format Report** holds the research report.

## Customizing

- **Find the right talks:** try `"<startup>" demo day`, `"<founder>" interview`, or `"<startup>" fundraise`.
- **Depth:** raise `maxItems` on **Keep Top 4**.

## Cost

Per run = **1 credit** for the search + up to **1 credit per transcribed talk** + one LLM call.

## Notes

- This is a **research aid**, not advice — it extracts only what's stated on camera and is instructed not to invent figures. Verify every claim against primary sources before any decision.
