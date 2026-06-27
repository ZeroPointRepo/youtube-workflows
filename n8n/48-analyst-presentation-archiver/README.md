# 48 · Analyst Presentation Archiver

Archive a research / IR channel's presentations into structured rows. It lists the channel's videos, transcribes each, and extracts a finance-research record — company, speaker, event type, summary, key statements, risks, and source URLs — ready for Notion, Airtable, or a spreadsheet.

```
Start → Set Channel → Resolve Channel → List Channel Videos → Extract Video List → Keep First 5
      → Get Transcript (TranscriptAPI) → Build Presentation Input → Has Transcript? → Archive Fields (LLM) → Format Archive Row
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Channel** | Edit Fields | The IR / research channel (`channel`). |
| **Resolve Channel** | HTTP Request | `GET /api/v2/youtube/channel/resolve`. |
| **List Channel Videos** | HTTP Request | `GET /api/v2/youtube/channel/videos`. |
| **Extract Video List** | Code | One item per presentation. |
| **Keep First 5** | Limit | Caps how many to archive (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Build Presentation Input** | Code (per item) | Joins the transcript + carries title/URLs. |
| **Has Transcript?** | Filter | Drops videos without captions. |
| **Archive Fields (LLM)** | HTTP Request | Returns `{ company, speaker, event_type, summary, key_statements, risks }` JSON. |
| **Format Archive Row** | Code (per item) | One archive row per presentation (incl. video + transcript-API URLs). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `analyst-presentation-archiver.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Channel** and set the IR/research channel.
5. Click **Test workflow**. **Format Archive Row** outputs one row per presentation.

## Customizing

- **Store it:** add a Notion / Airtable / Sheets node after **Format Archive Row** (no external write is required to *import* this workflow).
- **Volume:** raise `maxItems` on **Keep First 5**.

## Cost

`channel/videos` 1 credit/page + up to 1 credit per captioned presentation + one LLM call each.

## Notes

- **Not investment advice.** Fields are AI-extracted from public video transcripts and the prompt is told never to invent figures — always verify `key_statements`/`risks` against the official filing/record before relying on them. `company`/`speaker` may be `null` when not stated.
