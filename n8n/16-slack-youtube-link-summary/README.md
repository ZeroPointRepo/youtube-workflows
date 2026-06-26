# 16 · Slack Auto-Summary of Shared YouTube Links

When someone drops a YouTube link in Slack, this workflow grabs the transcript, summarizes it in 3 bullets, and posts the summary back to the channel. Great for team and community Slack workspaces that share a lot of videos.

```
Slack/Webhook Trigger → Extract YouTube URL → Get Transcript (TranscriptAPI) → Build Transcript Text → Summarize (LLM) → Post to Slack
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Slack/Webhook Trigger** | Webhook | Receives the Slack event (or any POST carrying a YouTube URL) at `/youtube-summary`. |
| **Extract YouTube URL** | Code | Finds the first YouTube link in the payload and reads the Slack channel ID. No link → does nothing. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (*continue on error*). |
| **Build Transcript Text** | Code | Joins the transcript and carries the title + channel forward. |
| **Summarize (LLM)** | HTTP Request | Produces a 3-bullet summary. |
| **Post to Slack** | HTTP Request | Posts back via `chat.postMessage` to the originating channel. |

## Prerequisites

- An [n8n](https://n8n.io) instance (reachable by Slack — cloud or a public URL).
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.
- A **Slack** app with a bot token (`chat:write`) and Event Subscriptions enabled.

## Setup

1. **Import** `slack-youtube-link-summary.json`.
2. Create three **Header Auth** credentials and attach each to its node:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
   | `Slack - Authorization Bearer` | `Authorization` | `Bearer YOUR_SLACK_BOT_TOKEN` |
3. **Activate** the workflow and copy the **production webhook URL** from the **Slack/Webhook Trigger** node.
4. In your Slack app → **Event Subscriptions**, set the Request URL to that webhook and subscribe to `message.channels` (and/or `app_mention`). Invite the bot to the channel.
5. Drop a YouTube link in the channel — the bot replies with a 3-bullet summary.

## Customizing

- **Bullets / format:** edit the **Summarize (LLM)** system prompt.
- **Trigger style:** works with a Slack slash command or `app_mention` too — just point them at the same webhook.

## Cost

Per shared link = **1 TranscriptAPI credit** (if the video has captions) + one LLM call. Messages without a YouTube link cost nothing.

## Notes

- **Slack URL verification:** Slack's one-time `url_verification` challenge must be answered when you first add the Request URL. Complete it with n8n's Slack Trigger node (or a temporary node that echoes `challenge`); afterwards this webhook handles message events normally.
- The URL extractor handles Slack's `<https://…|label>` link formatting and `youtu.be` / `watch` / `shorts` URLs.
