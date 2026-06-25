# YouTube Workflows ⚙️

> Ready-to-import automation workflows that turn YouTube videos into transcripts, summaries, and structured data — powered by [TranscriptAPI](https://transcriptapi.com).

Drop these into [n8n](https://n8n.io) (more platforms coming) and you've got a working pipeline in minutes: **YouTube → transcript → AI summary → wherever you want it** (Notion, Sheets, Slack, your database). No yt-dlp (YouTube blocks all major cloud IPs), no headless browsers, no scraping — just one fast API call. Powered by [TranscriptAPI](https://transcriptapi.com), the same backend behind [YouTubeToTranscript.com](https://youtubetotranscript.com).

**Free tier · No credit card · 100 credits on signup**

[TranscriptAPI.com](https://transcriptapi.com) · [API Docs](https://transcriptapi.com/docs/api/) · [Agent Skills](https://github.com/ZeroPointRepo/youtube-skills) · [MCP Server](https://github.com/ZeroPointRepo/youtube-mcp)

---

## What's Inside

| # | Workflow | Platform | What it does |
|---|----------|----------|--------------|
| 01 | [Single Video → Transcript → AI Summary](n8n/01-single-video-transcript-summary) | n8n | Paste one YouTube URL, get a clean transcript and a 5-bullet AI summary. The simplest way to see TranscriptAPI work. |
| 02 | [Channel → Latest Videos → Summaries → Notion](n8n/02-channel-latest-videos-to-notion) | n8n | Every day, grab a channel's newest videos, transcribe each, summarize with an LLM, and append a row to Notion. Set-and-forget content monitoring. |

More workflows (Make.com blueprints, Slack digests, RAG ingestion, translation) are on the way — [contributions welcome](CONTRIBUTING.md).

---

## Setup — Your TranscriptAPI Key

Every workflow needs a TranscriptAPI key for the transcript step.

1. Sign up at **[transcriptapi.com](https://transcriptapi.com)** — you get **100 free credits, no credit card**.
2. Copy your API key from the dashboard.
3. In n8n, create a **Header Auth** credential (used by the HTTP Request nodes):
   - **Name:** `Authorization`
   - **Value:** `Bearer YOUR_API_KEY`  ← keep the word `Bearer ` and the space

That's it. Each request costs **1 credit on success only** — failed, not-found, and rate-limited requests cost **0 credits**, so you never pay for a video that has no captions.

> 🔐 **Never paste your key into the workflow JSON.** Always use an n8n credential. The files in this repo only contain `REPLACE_WITH_...` placeholders.

---

## How to Import an n8n Workflow

1. Open n8n → **Workflows** → **Import from File** (or paste the JSON).
2. Pick the `.json` file from the workflow's folder.
3. Open each **HTTP Request** node and attach the right credential (see each workflow's README).
4. Replace any `REPLACE_WITH_...` placeholder (channel ID, Notion database ID, etc.).
5. Click **Test workflow** to run it once.

---

## How TranscriptAPI Works (quick reference)

All workflows call one endpoint:

```http
GET https://transcriptapi.com/api/v2/youtube/transcript?video_url=VIDEO_URL_OR_ID&format=json
Authorization: Bearer YOUR_API_KEY
```

`video_url` accepts a full watch URL, a `youtu.be` short link, or a bare 11-character video ID.

**Response:**

```json
{
  "video_id": "dQw4w9WgXcQ",
  "language": "en",
  "transcript": [
    { "text": "We're no strangers to love", "start": 18.2, "duration": 3.1 }
  ]
}
```

Add `&send_metadata=true` to also get the title, author, and thumbnail. Full reference: **[transcriptapi.com/docs/api](https://transcriptapi.com/docs/api/)**.

**Credits:** charged only on `HTTP 200`. `4xx`, `5xx`, and `429` cost nothing.

---

## Supported Platforms

| Platform | Status |
|----------|--------|
| [n8n](https://n8n.io) | ✅ Available |
| [Make.com](https://make.com) | 🔜 Planned |
| [Zapier](https://zapier.com) | 🔜 Planned |

Want one sooner? [Open an issue](https://github.com/ZeroPointRepo/youtube-workflows/issues) or send a PR.

---

## FAQ

**Do I need a YouTube API key?**
No. Workflow 02 reads a channel's public RSS feed (no key, no quota), and transcripts come from TranscriptAPI.

**What if a video has no transcript?**
The request returns a non-200 and costs 0 credits. Workflow 02 is built to skip those videos and keep going.

**Can I use a different LLM?**
Yes. The summarize step is a plain HTTP Request to an OpenAI-compatible chat endpoint — point it at OpenRouter, Groq, a local model, or swap in n8n's native AI nodes.

**Can I send results somewhere other than Notion?**
Absolutely. The transcript + summary half of each workflow is the reusable part — swap the final node for Google Sheets, Airtable, Slack, a webhook, or your database.

**Is this free?**
The workflows are MIT-licensed and free. TranscriptAPI has a free tier (100 credits); paid plans start at **$5**. See [pricing](https://transcriptapi.com/#pricing).

---

## Contributing

Got a useful workflow? See [CONTRIBUTING.md](CONTRIBUTING.md). The one hard rule: **no secrets in the files — placeholders and credential references only.**

## License

[MIT](LICENSE) © 2026 TranscriptAPI
