# 01 · Single Video → Transcript → AI Summary

The simplest TranscriptAPI workflow. Give it one YouTube URL and it returns a clean transcript plus a 5-bullet AI summary. Great as a starting point you can clone and extend.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Summarize (LLM) → Extract Summary
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The YouTube URL to process (edit this). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` with your TranscriptAPI Bearer key. |
| **Build Transcript Text** | Code | Joins the transcript segments into one plain-text string. |
| **Summarize (LLM)** | HTTP Request | POSTs the transcript to an OpenAI-compatible chat endpoint. |
| **Extract Summary** | Edit Fields | Pulls the summary text out of the LLM response. |

## Prerequisites

- An [n8n](https://n8n.io) instance (cloud or self-hosted).
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key for the summary step.

## Setup

1. **Import** `single-video-transcript-summary.json` (Workflows → Import from File).
2. Create two **Header Auth** credentials in n8n:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach each credential to its HTTP Request node (the import leaves them as `REPLACE_..._CRED_ID` until you pick yours).
4. Open **Set Video URL** and paste the YouTube URL you want.
5. Click **Test workflow**. The **Extract Summary** node's output holds your summary.

## Customizing

- **Different model / provider:** edit the `model` field and URL in **Summarize (LLM)** — any OpenAI-compatible endpoint works.
- **Want the raw transcript?** It's already on **Build Transcript Text** as `transcriptText`.
- **Send it somewhere:** add a node after **Extract Summary** (Slack, email, Notion, a webhook…).

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + your LLM's per-call cost.
