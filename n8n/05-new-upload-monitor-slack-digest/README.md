# 05 · New-Upload Monitor → Key Points → Slack Digest

Watch a channel (a competitor, a partner, an industry voice) and get a Slack digest whenever it posts. On a schedule it checks the channel's newest upload, and — only if that upload is genuinely new — transcribes it, pulls the key takeaways with an LLM, and posts them to Slack. No YouTube API key required.

```
Every 6 Hours → Set Channel → Get Latest Videos (TranscriptAPI) → Pick Newest Video
              → Is New Upload? → Get Transcript (TranscriptAPI) → Build Transcript Text
              → Extract Key Points (LLM) → Send Slack Digest
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Every 6 Hours** | Schedule Trigger | Runs on an interval (default every 6h; click **Test workflow** to run now). |
| **Set Channel** | Edit Fields | The channel to watch — an `@handle`, channel URL, or `UC…` ID. |
| **Get Latest Videos (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/channel/latest` — newest 15 uploads via RSS. **Free** (no credit charged). |
| **Pick Newest Video** | Code | Takes the most recent upload and flags whether it was published within the last 24h. |
| **Is New Upload?** | Filter | Stops here unless the newest video is genuinely new — so re-runs don't re-post old videos. |
| **Get Transcript (TranscriptAPI)** | HTTP Request | Fetches the transcript (*continue on error* if captions aren't ready yet). |
| **Build Transcript Text** | Code | Joins segments and carries the title/URL/channel forward. |
| **Extract Key Points (LLM)** | HTTP Request | Summarizes the video into 5–7 key bullets. |
| **Send Slack Digest** | HTTP Request | Posts the digest to Slack via `chat.postMessage`. |

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.
- A **Slack** bot token (`xoxb-…`) with `chat:write`, and the target channel's ID.

## Setup

1. **Import** `new-upload-monitor-slack-digest.json`.
2. Create three **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
   | `Slack - Authorization Bearer` | `Authorization` | `Bearer YOUR_SLACK_BOT_TOKEN` |
3. Attach each credential to its HTTP Request node.
4. Open **Set Channel** and set the channel (`@handle`, URL, or `UC…` ID) in place of `REPLACE_WITH_CHANNEL_HANDLE_OR_ID`.
5. In **Send Slack Digest**, replace `REPLACE_WITH_SLACK_CHANNEL_ID` with your Slack channel ID (e.g. `C0123456789`).
6. Click **Test workflow** to try it now, or **Activate** the workflow to let the schedule run it.

## Customizing

- **Cadence:** edit **Every 6 Hours** (hourly, daily, etc.). Keep the lookback window in **Pick Newest Video** ≥ your interval so nothing slips through.
- **"New" window:** change `LOOKBACK_HOURS` in **Pick Newest Video**.
- **Email instead of Slack:** swap **Send Slack Digest** for an email/Gmail/SendGrid node — everything upstream stays the same.
- **Watch several channels:** duplicate the chain, or feed multiple channels through the loop.

## Cost

Per matching run = **0 credits** for the channel check (free endpoint) + **1 credit** for the transcript (only if captions exist) + one LLM call. Runs where there's no new upload cost **0 credits** and post nothing.

## Notes

- The "new upload" gate is **stateless** — it relies on the publish timestamp, not stored history — so it works without a database. If you prefer exact de-duplication, add n8n's **Remove Duplicates** node keyed on `videoId`.
- A brand-new upload may not have captions for a few minutes; the transcript node is set to continue on error so the workflow won't crash.
