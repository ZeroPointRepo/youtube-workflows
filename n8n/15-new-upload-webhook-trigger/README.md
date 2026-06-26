# 15 · New-Upload → Webhook Trigger

A lightweight trigger for automation pipelines: poll a channel on a schedule and fire a webhook **only when a new video is published**. It de-duplicates on the last-seen video ID (stored in the workflow's own state — no database), and it runs entirely on the **free** `channel/latest` endpoint, so it costs **0 credits**.

```
Every 15 Minutes → Set Config → Get Latest Videos (TranscriptAPI) → Detect New Upload → Fire Webhook
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every 15 Minutes** | Schedule Trigger | Polls on an interval (default 15 min). |
| **Set Config** | Edit Fields | The `channel` to watch and the `webhookUrl` to call. |
| **Get Latest Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/channel/latest` — **free**, newest-first. |
| **Detect New Upload** | Code | Fires only if the newest video ID differs from the last seen one (persisted in workflow static data). |
| **Fire Webhook** | HTTP Request | POSTs a JSON payload (`event`, `videoId`, `title`, `url`, …) to your `webhookUrl`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- A destination **webhook URL** (Zapier, Make, your own endpoint, Slack incoming webhook, etc.).

## Setup

1. **Import** `new-upload-webhook-trigger.json`.
2. Create one **Header Auth** credential and attach it to **Get Latest Videos**:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Open **Set Config** and set `channel` (`@handle`, URL, or `UC…` ID) and `webhookUrl` (replace `REPLACE_WITH_YOUR_WEBHOOK_URL`).
4. **Activate** the workflow so the schedule runs.

## Customizing

- **Cadence:** edit **Every 15 Minutes** (seconds/minutes/hours).
- **Add a summary to the payload:** insert a **Get Transcript** node + an LLM node between **Detect New Upload** and **Fire Webhook**, then include the summary in the webhook body (this adds 1 credit per new video).
- **Auth on the webhook:** add a header credential to **Fire Webhook** if your endpoint needs one.

## Cost

**0 credits** as shipped — `channel/latest` is free. Adding an optional transcript step costs 1 credit per new video.

## Notes

- De-dup uses **workflow static data** (`$getWorkflowStaticData`), which persists across executions — so the webhook fires once per genuinely new upload. The very first run fires for the current latest video and records it as the baseline.
