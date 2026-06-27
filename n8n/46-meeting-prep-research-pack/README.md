# 46 · Meeting Prep Research Pack

Walk into any meeting prepared. Give it a company name and it searches that company's videos, transcribes the most relevant ones, and produces a meeting-prep brief — company narrative, recent announcements, the customer pain points they target, likely objections, and good discovery questions to ask.

```
Start → Set Company → Search Videos (TranscriptAPI) → Extract Captioned Results → Keep Top 4
      → Get Transcript (TranscriptAPI) → Collect Transcripts → Meeting Prep Brief (LLM) → Format Brief
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Company** | Edit Fields | The company to research (`searchQuery`). |
| **Search Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/search?q=…&type=video`. |
| **Extract Captioned Results** | Code | Keeps captioned results; one item per video. |
| **Keep Top 4** | Limit | Caps how many videos to transcribe (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Collect Transcripts** | Code | Combines into one corpus. |
| **Meeting Prep Brief (LLM)** | HTTP Request | Returns `{ company_narrative, recent_announcements, customer_pain_points, likely_objections, discovery_questions }` JSON. |
| **Format Brief** | Code | Clean brief fields + the `sources` list. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `meeting-prep-research-pack.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Company** and enter the company name.
5. Click **Test workflow**. **Format Brief** holds the meeting-prep pack.

## Customizing

- **Tighter results:** try queries like `"<company> webinar"`, `"<company> demo"`, or `"<company> founder"`.
- **Scope to one channel:** swap the search for `channel/search` (`channel` + `q`) when you know the company's channel.
- **Depth:** raise `maxItems` on **Keep Top 4**.

## Cost

Per run = **1 credit** for the search + up to **1 credit per transcribed video** + one LLM call.

## Notes

- Grounded only in the transcripts (the prompt forbids inventing facts) — verify anything material against primary sources before the meeting.
