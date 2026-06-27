# AI Agent Skills

Ready-to-use **skill files** for AI agents (Claude, Codex, OpenClaw, Hermes, and any
agent that reads skill/markdown files). Each one teaches your agent to do a YouTube
task using **[TranscriptAPI](https://transcriptapi.com)** to fetch transcripts —
**your agent does the thinking itself**, so all you need is a TranscriptAPI key
(no separate AI/LLM key).

These mirror the [n8n workflows](../n8n) in this repo, for the AI-agent world.

## How to use one

1. Open the skill file for the task you want (list below) and download it.
2. Add it to your agent (drop it in wherever your agent reads skills/instructions).
3. Give the agent your **TranscriptAPI key** — [get a free one](https://transcriptapi.com/signup) (100 credits, no credit card).
4. Ask the agent to run the task. It calls TranscriptAPI for the transcript and reasons over it natively.

The TranscriptAPI call each skill uses:
```
GET https://transcriptapi.com/api/v2/youtube/transcript?video_url=<YOUTUBE_URL>
Authorization: Bearer <YOUR_TRANSCRIPTAPI_KEY>
```

## The skills (34)

- **[Brand-Mention Monitor → Slack Digest](./brand-mention-monitor-slack.md)** — Track a brand or keyword on YouTube with a daily Slack digest.
- **[Bulk Transcript Processor (with Rate-Limit Handling)](./bulk-transcript-processor.md)** — Reliably fetch transcripts for a big batch of videos.
- **[Channel → Latest Videos → AI Summaries → Notion](./channel-latest-videos-to-notion.md)** — Auto-summarize a channel's newest videos into Notion.
- **[Channel Sentiment Pipeline](./channel-sentiment-pipeline.md)** — Score a channel's latest videos for sentiment and topics.
- **[Channel → Full Video Metadata DB](./channel-video-metadata-db.md)** — Build a clean database of a channel's videos.
- **[Competitor Analysis Agent](./competitor-analysis-agent.md)** — Get a competitor-intelligence report from their YouTube.
- **[Earnings-Call Tracker → Analyst Summary → Slack](./earnings-call-tracker.md)** — Get an analyst summary when a company posts an earnings call.
- **[Video → Transcript → Key Quotes (with Timestamps)](./key-quotes-extractor.md)** — Pull the most quotable moments, with timestamps.
- **[Market Commentary Daily Brief](./market-commentary-daily-brief.md)** — Get a daily market brief from finance YouTube channels.
- **[Multi-Channel Content Database Ingester](./multi-channel-content-db.md)** — Build one content database from many channels.
- **[Multi-Video Topic Researcher → Research Brief](./multi-video-topic-researcher.md)** — Research a topic across YouTube and get one tidy brief.
- **[New Course Video → Auto Study Notes → Notion](./new-course-video-notes.md)** — Turn new course videos into study notes in Notion.
- **[New-Upload Monitor → Key Points → Slack Digest](./new-upload-monitor-slack-digest.md)** — Get a Slack digest of key points whenever a channel posts.
- **[New-Upload → Webhook Trigger](./new-upload-webhook-trigger.md)** — Fire a webhook the moment a channel posts a new video.
- **[Playlist → Full Text Corpus (CSV / JSON)](./playlist-full-text-corpus.md)** — Turn a playlist into a full-text dataset (CSV/JSON).
- **[Playlist → Quiz / Flashcard Generator](./playlist-quiz-flashcards.md)** — Turn a lecture playlist into quizzes and flashcards.
- **[Playlist → Study Notes Generator](./playlist-study-notes.md)** — Turn a course playlist into per-video study notes.
- **[Playlist / Channel → Transcripts → RAG Chunks](./playlist-to-rag-chunks.md)** — Turn a playlist or channel into clean text chunks for AI search.
- **[Share-of-Voice Analyzer](./share-of-voice-analyzer.md)** — Measure how much of a topic your brand owns vs rivals.
- **[Single Video → Transcript → AI Summary](./single-video-transcript-summary.md)** — Turn one YouTube video into a clean transcript and a 5-bullet summary.
- **[Slack Auto-Summary of Shared YouTube Links](./slack-youtube-link-summary.md)** — Auto-summarize YouTube links shared in Slack.
- **[Student Q&A → Video Timestamp Lookup](./student-qa-timestamp-lookup.md)** — Answer a question and point to the exact video moment.
- **[Topic Trend Tracker](./topic-trend-tracker.md)** — Track a topic's momentum on YouTube over time.
- **[Video → Short-Form Script Pack](./video-short-form-script-pack.md)** — Turn one long video into 3–5 short-form scripts.
- **[Video → Transcript → Content Classifier](./video-to-content-classifier.md)** — Auto-tag a video with category, topics, and audience.
- **[Video-to-Embedding Batch Processor](./video-to-embedding-batch.md)** — Turn a batch of videos into vector embeddings.
- **[Video → Transcript → LinkedIn Post](./video-to-linkedin-post.md)** — Turn a video into a ready-to-post LinkedIn update.
- **[Video → Transcript → Newsletter Section](./video-to-newsletter-section.md)** — Turn a video into a polished newsletter section.
- **[Video → Transcript → Podcast Show Notes + Chapters](./video-to-podcast-show-notes.md)** — Generate show notes and chapters for any episode.
- **[Single Video → Transcript → SEO Blog-Post Draft](./video-to-seo-blog-post.md)** — Turn any video into a publish-ready SEO blog post.
- **[Video → Transcript → X (Twitter) Thread](./video-to-x-thread.md)** — Turn a video into a ready-to-post 10-tweet X thread.
- **[Video → Transcript → Translate → Multilingual Summary](./video-translate-multilingual-summary.md)** — Get any video's gist, translated into the language you choose.
- **[Weekly Industry Brief across Channels](./weekly-industry-brief.md)** — Get one weekly executive brief across many channels.
- **[YouTube SEO Title & Description Analyzer](./youtube-seo-analyzer.md)** — See what's ranking for a keyword and how to beat it.

---

Free & open source (MIT). Prefer no-code? Every skill also ships as an [n8n workflow](../n8n).
