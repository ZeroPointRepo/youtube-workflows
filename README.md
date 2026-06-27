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
| 03 | [Video → Transcript → SEO Blog-Post Draft](n8n/03-video-to-seo-blog-post) | n8n | Repurpose one video into a publish-ready blog draft — SEO title, meta description, slug, tags, and a full Markdown article. For content marketers. |
| 04 | [Playlist / Channel → Transcripts → RAG Chunks](n8n/04-playlist-to-rag-chunks) | n8n | Transcribe a whole playlist and emit clean, overlapping `{id, text, metadata}` chunks ready to embed into a vector DB. For RAG / agent builders. |
| 05 | [New-Upload Monitor → Key Points → Slack](n8n/05-new-upload-monitor-slack-digest) | n8n | Watch a channel and get a Slack digest of key takeaways whenever it posts something new. For teams tracking channels/competitors. |
| 06 | [Video → Transcript → Translate → Multilingual Summary](n8n/06-video-translate-multilingual-summary) | n8n | Translate + summarize any video into a target language (translated title, bullet summary, key topics). For global creators and localizers. |
| 07 | [Multi-Video Topic Researcher → Research Brief](n8n/07-multi-video-topic-researcher) | n8n | Search a topic, transcribe the top results, and synthesize a structured research brief (themes, consensus, disagreements). For research/AI. |
| 08 | [Video → LinkedIn Post](n8n/08-video-to-linkedin-post) | n8n | Repurpose one video into a ready-to-post LinkedIn update — hook, body, CTA, hashtags. For content repurposers. |
| 09 | [Video → X (Twitter) Thread](n8n/09-video-to-x-thread) | n8n | Repurpose one video into a numbered 10-tweet X thread (hook → ideas → CTA). For content repurposers. |
| 10 | [Brand-Mention Monitor → Slack Digest](n8n/10-brand-mention-monitor-slack) | n8n | Daily search for a brand/keyword, transcribe the new results, and post a Slack digest of mentions. For marketers / competitive intel. |
| 11 | [Video → Content Classifier](n8n/11-video-to-content-classifier) | n8n | Auto-tag a video with category, content type, topics, and audience — structured for an Airtable/Notion content database. |
| 12 | [Video → Newsletter Section](n8n/12-video-to-newsletter-section) | n8n | Turn a video into a polished newsletter section (headline + insights + takeaway), Substack/Beehiiv-ready. |
| 13 | [Video → Key Quotes (with Timestamps)](n8n/13-key-quotes-extractor) | n8n | Extract 5–10 tweetable quotes with timestamps, one row per quote — ready for Airtable. |
| 14 | [Video → Podcast Show Notes + Chapters](n8n/14-video-to-podcast-show-notes) | n8n | Generate a summary, timestamped chapter markers, and full Markdown show notes for any episode. |
| 15 | [New-Upload → Webhook Trigger](n8n/15-new-upload-webhook-trigger) | n8n | Poll a channel and fire a webhook only on a genuinely new upload (last-seen-ID de-dupe). Free-endpoint based. For automation operators. |
| 16 | [Slack Auto-Summary of Shared YouTube Links](n8n/16-slack-youtube-link-summary) | n8n | When a YouTube link is posted in Slack, reply with a 3-bullet summary. For internal/community Slack workspaces. |
| 17 | [New Course Video → Auto Study Notes → Notion](n8n/17-new-course-video-notes) | n8n | When a course channel posts a lecture, generate structured study notes and save them to Notion. For edtech/students. |
| 18 | [Weekly Industry Brief across Channels](n8n/18-weekly-industry-brief) | n8n | Monitor 5–10 channels and post one weekly executive brief from the new videos. For marketers/analysts. |
| 19 | [Channel → Full Video Metadata DB](n8n/19-channel-video-metadata-db) | n8n | Resolve a channel, list its uploads, and emit one metadata row per video — ready for Sheets/Airtable/a database. |
| 20 | [Playlist → Full Text Corpus (CSV/JSON)](n8n/20-playlist-full-text-corpus) | n8n | Turn a playlist into a structured corpus: one row per video with metadata + full transcript text. For research/datasets. |
| 21 | [Playlist → Quiz / Flashcard Generator](n8n/21-playlist-quiz-flashcards) | n8n | Transcribe lectures and generate Anki-style flashcards + quiz questions, one row per card. For edtech/students. |
| 22 | [Earnings-Call Tracker → Analyst Summary → Slack](n8n/22-earnings-call-tracker) | n8n | Watch an IR channel; on a new earnings call, transcribe it and post an analyst summary to Slack. For investors/analysts. |
| 23 | [Playlist → Study Notes Generator](n8n/23-playlist-study-notes) | n8n | Turn a lecture playlist into structured study notes per video (summary, key concepts, definitions, timestamps). For edtech/students. |
| 24 | [Market Commentary Daily Brief](n8n/24-market-commentary-daily-brief) | n8n | Monitor finance/macro channels and post a daily market brief (themes, tickers, macro, risks) to Slack. For finance/analysts. |
| 25 | [Bulk Transcript Processor (rate-limit handling)](n8n/25-bulk-transcript-processor) | n8n | Batch-process many video URLs with batching, retry/backoff, and success/failure rows. Credit-safe. For data/ops teams. |
| 26 | [Video → Short-Form Script Pack](n8n/26-video-short-form-script-pack) | n8n | Turn one long video into 3–5 Reels/Shorts/TikTok scripts (hook, beats, caption, CTA). For content creators. |
| 27 | [Competitor Analysis Agent](n8n/27-competitor-analysis-agent) | n8n | Search a competitor, transcribe top videos, and generate an intel report (positioning, claims, themes, objections, quotes). For marketers. |
| 28 | [YouTube SEO Title & Description Analyzer](n8n/28-youtube-seo-analyzer) | n8n | Analyze top results for a keyword and get SEO patterns + title/description recommendations. For creators/marketers. |
| 29 | [Student Q&A → Video Timestamp Lookup](n8n/29-student-qa-timestamp-lookup) | n8n | Ask a question against a course channel; get a concise answer with the source video and timestamps. For edtech/students. |
| 30 | [Multi-Channel Content Database Ingester](n8n/30-multi-channel-content-db) | n8n | Resolve a list of channels, list their videos, and emit unified content-database rows. Low-credit, no transcripts. For data teams. |
| 31 | [Video-to-Embedding Batch Processor](n8n/31-video-to-embedding-batch) | n8n | Transcribe a batch of videos, chunk, and generate embeddings as Supabase pgvector-ready rows. For AI/RAG builders. |
| 32 | [Channel Sentiment Pipeline](n8n/32-channel-sentiment-pipeline) | n8n | Score a channel's latest videos for sentiment + topics, one row per video. For analysts/marketers. |
| 33 | [Topic Trend Tracker](n8n/33-topic-trend-tracker) | n8n | Weekly keyword search snapshots (result count + top-video metadata) to chart a topic over time. Low-credit. For marketers. |
| 34 | [Share-of-Voice Analyzer](n8n/34-share-of-voice-analyzer) | n8n | Compare brand vs competitor presence in YouTube search results — rank, count, share-%. Search-only, low-credit. For marketers. |
| 35 | [Ask-a-Playlist Chatbot Builder](n8n/35-ask-a-playlist-chatbot) | n8n | Transcribe a playlist and emit a chatbot knowledge base (config + KB chunks) for a Q&A agent. For AI builders. |
| 36 | [Weekly Channel Digest → Ghost/Substack Draft](n8n/36-weekly-channel-digest) | n8n | Summarize a channel's week and post a ready-to-edit newsletter draft (placeholder publish). For creators. |
| 37 | [Product Launch Watcher](n8n/37-product-launch-watcher) | n8n | Watch competitor channels and Slack-alert when new titles match launch/release keywords. Free, no transcripts. For marketers. |
| 38 | [Influencer Vetting Pipeline](n8n/38-influencer-vetting-pipeline) | n8n | Resolve a creator, sample transcripts, and score brand-safety/fit with rationale + flags. For marketers/brands. |
| 39 | [Speaker Research Brief](n8n/39-speaker-research-brief) | n8n | Search a speaker's talks and build a one-page brief — themes, talking points, timestamped evidence. For event/PR teams. |
| 40 | [Startup Pitch Research Tool](n8n/40-startup-pitch-research) | n8n | Research a startup from founder talks — claims, traction signals, risks, diligence questions. For investors/analysts. |
| 41 | [Policy Speech Monitor](n8n/41-policy-speech-monitor) | n8n | Watch a policy/government channel and post a neutral Slack briefing when it publishes. For policy/comms teams. |
| 42 | [Onboarding Video → Internal Wiki Page](n8n/42-onboarding-video-wiki) | n8n | Turn a training video into a Confluence/Notion-style wiki page (summary, steps, links, FAQ). For internal ops/L&D. |

More workflows (Make.com blueprints, Zapier zaps, and many more n8n recipes) are on the way — [contributions welcome](CONTRIBUTING.md).

### 🤖 Also available as AI Agent Skills

Every workflow above also ships as an **[AI agent skill](agent-skills/)** — a markdown skill file for Claude, Codex, OpenClaw, Hermes, and any agent that reads skill files. Same task, but your agent does the reasoning itself, so all you need is a TranscriptAPI key (**no separate AI/LLM key**). Browse all 34 in **[`agent-skills/`](agent-skills/)**.

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
