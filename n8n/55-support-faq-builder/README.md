# 55 · Support FAQ Builder from Tutorials

Turn your tutorial/how-to channel into a draft support FAQ. Point it at a channel and it lists the videos, transcribes them, and drafts FAQ entries — question, answer, category, and a confidence score — each linked back to the source video. Built for a support/knowledge-base team to review and publish.

```
Start → Set Channel → Resolve Channel → List Channel Videos → Extract Video List → Keep First 5
      → Get Transcript (TranscriptAPI) → Build FAQ Input → Has Transcript? → FAQ Extract (LLM) → Format FAQ Rows
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Channel** | Edit Fields | Your tutorial/support channel (`channel` — handle or ID). |
| **Resolve Channel** | HTTP Request | `GET /api/v2/youtube/channel/resolve`. |
| **List Channel Videos** | HTTP Request | `GET /api/v2/youtube/channel/videos`. |
| **Extract Video List** | Code | One item per video. |
| **Keep First 5** | Limit | Caps how many to transcribe (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Build FAQ Input** | Code (per item) | Joins transcript + carries title/URLs. |
| **Has Transcript?** | Filter | Drops videos without captions. |
| **FAQ Extract (LLM)** | HTTP Request | Drafts `faqs:[{question, answer, category, confidence}]` per video. |
| **Format FAQ Rows** | Code (per item) | Flattens to one row per FAQ, each tagged `status: "draft"` + source video. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `support-faq-builder.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to the three TranscriptAPI nodes and OpenAI to **FAQ Extract**.
4. Open **Set Channel** and set your tutorial channel.
5. Click **Test workflow**. **Format FAQ Rows** holds the draft FAQ entries.

## Customizing

- **Publish queue:** add a Notion / Zendesk / Help-Scout / Sheets node after **Format FAQ Rows** — but keep a human approval step (entries are drafts).
- **Volume:** raise `maxItems` on **Keep First 5**.
- **Filter by confidence:** route only `confidence: "high"` rows to a faster review lane.

## Cost

`channel/videos` 1 credit/page + up to 1 credit per captioned video + one LLM call each.

## Notes

- **Internal drafts — human review required.** Every entry is `status: "draft"` and is grounded only in what the video says; the prompt is told never to invent product behavior. **Review and edit before publishing** any answer to customers.
