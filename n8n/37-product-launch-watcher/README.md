# 37 · Product Launch Watcher

Watch competitor channels and get a Slack alert the moment they post a launch, release, or announcement. It checks each channel's newest videos daily, matches titles against launch keywords, de-duplicates across runs, and pings you with what's new. Free-endpoint based — no transcripts.

```
Every Day → Set Channels → Get Latest Videos (TranscriptAPI) → Detect Launches → Send Alert
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every Day** | Schedule Trigger | Checks daily at 09:00. |
| **Set Channels** | Code | The competitor channels to watch (edit the array). |
| **Get Latest Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/channel/latest` per channel — **free**. |
| **Detect Launches** | Code | Flags recent videos whose titles match launch keywords; de-dupes across runs via workflow static data. |
| **Send Alert** | HTTP Request | Posts a Slack digest of new launches (`chat.postMessage`). |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- A **Slack** bot token (`chat:write`) + channel ID (or swap for email/webhook).

## Setup

1. **Import** `product-launch-watcher.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `Slack - Authorization Bearer` | `Authorization` | `Bearer YOUR_SLACK_BOT_TOKEN` |
3. Attach TranscriptAPI to **Get Latest Videos** and Slack to **Send Alert**.
4. Edit **Set Channels** with the channels to watch.
5. In **Send Alert**, replace `REPLACE_WITH_SLACK_CHANNEL_ID`.
6. **Activate** the workflow.

## Customizing

- **Keywords:** edit the `KEYWORDS` list in **Detect Launches** (add your product names, "drop", "out now"…).
- **Window/cadence:** tune `WINDOW_DAYS` and the **Every Day** schedule.
- **Email instead of Slack:** swap **Send Alert** for an email node.

## Cost

**0 credits** as shipped — `channel/latest` is free and no transcripts are pulled. Runs with no new launches post nothing.

## Notes

- De-dup uses workflow static data (a capped set of alerted video IDs), so you won't get pinged twice for the same launch.
