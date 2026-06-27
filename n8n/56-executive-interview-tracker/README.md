# 56 · Executive Interview Tracker

Keep tabs on what an executive (or company spokesperson) is saying publicly. Give it a name and it searches their interviews and appearances, transcribes the top results, and produces a tracker entry — summary, key statements, notable claims to verify, risks, and smart follow-up questions.

```
Start → Set Executive → Search Videos (TranscriptAPI) → Extract Captioned Results → Keep Top 4
      → Get Transcript (TranscriptAPI) → Collect Transcripts → Interview Analysis (LLM) → Format Tracker
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Executive** | Edit Fields | The executive / company to track (`searchQuery`). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Extract Captioned Results** | Code | Keeps captioned results; one item per video. |
| **Keep Top 4** | Limit | Caps how many interviews to transcribe (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Collect Transcripts** | Code | Builds a `[m:ss]`-timestamped corpus. |
| **Interview Analysis (LLM)** | HTTP Request | Returns `{ summary, key_statements, notable_claims, risks, follow_up_questions }`. |
| **Format Tracker** | Code | Clean tracker entry + the `sources` list. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `executive-interview-tracker.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to **Interview Analysis**.
4. Open **Set Executive** and enter the name (try `"<name>" interview` or `"<name>" earnings call`).
5. Click **Test workflow**. **Format Tracker** holds the entry.

## Customizing

- **Schedule it:** swap **Start** for a Schedule Trigger to track an exec over time.
- **Sharper search:** refine the **Set Executive** query (add company, topic, or event).
- **Depth:** raise `maxItems` on **Keep Top 4**.

## Cost

Per run = **1 credit** for the search + up to **1 credit per transcribed interview** + one LLM call.

## Notes

- **Research aid — not advice.** This summarizes public statements; it is **not** investment advice and performs **no** trading, outreach, or automated actions. `notable_claims` are flagged precisely because they need verifying — always check against the official record before relying on anything.
