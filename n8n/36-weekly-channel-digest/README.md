# 36 · Weekly Channel Digest → Ghost/Substack Draft

Turn a channel's week into a ready-to-publish newsletter. Once a week it pulls the channel's new videos, transcribes them, summarizes each, assembles a Markdown digest post, and sends it to a **draft** endpoint (Ghost/Substack/your webhook) — as a draft only, never auto-published.

```
Every Week → Set Channel → Get Latest Videos (TranscriptAPI) → Collect New Videos → Keep Top 8
           → Get Transcript (TranscriptAPI) → Aggregate Digest Input → Write Weekly Digest (LLM) → Create Draft (placeholder)
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every Week** | Schedule Trigger | Runs weekly (every 7 days at 08:00). |
| **Set Channel** | Edit Fields | The `channel` to digest and the `draftUrl` to post the draft to. |
| **Get Latest Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/channel/latest` — **free**. |
| **Collect New Videos** | Code | Keeps videos published in the last 7 days. |
| **Keep Top 8** | Limit | Caps how many to summarize (= credit budget). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches each transcript (*continue on error*). |
| **Aggregate Digest Input** | Code | Combines transcripts into one corpus. |
| **Write Weekly Digest (LLM)** | HTTP Request | Writes the Markdown digest post. |
| **Create Draft (placeholder)** | HTTP Request | POSTs `{ status: "draft", title, markdown }` to your `draftUrl`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.
- A **draft endpoint** — your Ghost Admin API, a Substack/Make/Zapier webhook, or any URL that accepts a draft.

## Setup

1. **Import** `weekly-channel-digest.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to both TranscriptAPI nodes and OpenAI to the LLM node.
4. Open **Set Channel**: set `channel` and `draftUrl` (replace `REPLACE_WITH_GHOST_OR_SUBSTACK_DRAFT_URL`).
5. **Activate** the workflow (or **Test workflow** to generate a draft now).

## Customizing

- **Real publishing target:** point **Create Draft** at the Ghost Admin API (add the JWT auth header) or your platform's draft endpoint. As shipped it only creates a **draft** — review before publishing.
- **Digest style/length:** edit the **Write Weekly Digest** prompt.

## Cost

Per run = **0 credits** for the channel check (free) + up to **1 credit per transcribed video** (capped by **Keep Top 8**) + one LLM call.

## Notes

- The draft step is a placeholder by design — **no real publishing**. Wire it to your platform and keep a human in the loop before anything goes live.
