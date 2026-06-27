# 51 · YouTube Fact-Check Assistant

Turn a video into a claim-by-claim **review sheet** for a human fact-checker. Paste a video URL and it transcribes with timestamps, extracts the distinct checkable factual claims (stats, dates, events, attributions), and outputs one row per claim — each with a timestamp deep-link, a suggested way to verify it, and a blank `status: unverified` for your reviewer to fill in.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Timestamped Text → Extract Claims (LLM) → Format Review Sheet
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The video to review (`videoUrl`). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true`). |
| **Build Timestamped Text** | Code | Builds a `[m:ss]`/`[h:mm:ss]`-timestamped transcript so claims can cite where they were said. |
| **Extract Claims (LLM)** | HTTP Request | Extracts checkable claims only. Returns `{ claims:[{claim, claim_type, timestamp, check_suggestion}], overall_caveats }`. |
| **Format Review Sheet** | Code | One row per claim: `claim`, `claimType`, `timestamp`, `deepLink`, `checkSuggestion`, `status: "unverified"`, blank `reviewerVerdict`/`evidenceUrl`. |

## Output shape

Each row: `{ n, claim, claimType, timestamp, deepLink, checkSuggestion, status, reviewerVerdict, evidenceUrl, sourceTitle, videoUrl }`. The first row also carries `caveats`.

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).
- An **OpenAI** (or compatible) API key.

## Setup

1. **Import** `youtube-fact-check-assistant.json`.
2. Create two **Header Auth** credentials:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
   | `OpenAI - Authorization Bearer` | `Authorization` | `Bearer YOUR_OPENAI_KEY` |
3. Attach the TranscriptAPI credential to **Get Transcript** and OpenAI to **Extract Claims**.
4. Open **Set Video URL** and paste your video URL.
5. Click **Test workflow**. **Format Review Sheet** holds one row per claim.

## Customizing

- **Send it somewhere:** add a Notion / Airtable / Google Sheets node after **Format Review Sheet** to build a review queue (content only — no auto-publish by default).
- **Claim focus:** edit the **Extract Claims** prompt to target a claim type (e.g. only statistics, or only attributions).
- **Deep-links:** each `deepLink` jumps to the moment the claim was made (`&t=<seconds>s`).

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript) + one LLM call.

## Notes

- **This is an assistant for human review — not a truth oracle.** It only *extracts and organizes* claims; it never rates them true or false (status starts as `unverified`). A person must verify each claim against primary sources. The LLM is instructed to extract only what is stated and never to judge truthfulness, but always sanity-check the extraction too.
