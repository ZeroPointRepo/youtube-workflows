# 44 · Conference / Talk Archiver

Build a searchable archive of a conference channel's talks. It lists the channel's videos, transcribes each, and extracts structured archive rows — talk title, speaker, company, a summary, tags, and links — ready for Notion, Airtable, or a spreadsheet.

```
Start → Set Channel → Resolve Channel → List Channel Videos → Extract Video List → Keep First 5
      → Get Transcript (TranscriptAPI) → Build Talk Input → Has Transcript? → Archive Fields (LLM) → Format Archive Row
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Channel** | Edit Fields | The conference / talks channel (`channel`). |
| **Resolve Channel** | HTTP Request | `GET /api/v2/youtube/channel/resolve`. |
| **List Channel Videos** | HTTP Request | `GET /api/v2/youtube/channel/videos`. |
| **Extract Video List** | Code | One item per talk. |
| **Keep First 5** | Limit | Caps how many talks to archive (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Build Talk Input** | Code (per item) | Joins the transcript + carries title/URLs. |
| **Has Transcript?** | Filter | Drops talks without captions. |
| **Archive Fields (LLM)** | HTTP Request | Returns `{ speaker, company, summary, tags }` JSON. |
| **Format Archive Row** | Code (per item) | One archive row per talk (incl. video + transcript-API URLs). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `conference-talk-archiver.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Channel** and set the conference channel.
5. Click **Test workflow**. **Format Archive Row** outputs one row per talk.

## Customizing

- **Store it:** add a Notion / Airtable / Google Sheets node after **Format Archive Row** — the fields map cleanly to columns. (No external credential is required to *import* this workflow — the destination is yours to add.)
- **Volume:** raise `maxItems` on **Keep First 5**.
- **More fields:** edit the **Archive Fields** prompt (e.g. add "track", "level", "key takeaways").

## Cost

`channel/videos` 1 credit/page + up to 1 credit per captioned talk + one LLM call per talk.

## Notes

- `speaker`/`company` are best-effort from the transcript and may be `null` when not stated — review before relying on them.
