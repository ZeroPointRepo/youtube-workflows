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

## The skills (58)

- **[Analyst Presentation Archiver](./analyst-presentation-archiver.md)** — Archive a research or IR channel's presentations into rows.
- **[Ask-a-Playlist Chatbot Builder](./ask-a-playlist-chatbot.md)** — Build the knowledge base for a chatbot that answers questions about a playlist.
- **[Brand-Mention Monitor → Slack Digest](./brand-mention-monitor-slack.md)** — Track a brand or keyword on YouTube with a daily Slack digest.
- **[Bulk Transcript Processor (with Rate-Limit Handling)](./bulk-transcript-processor.md)** — Reliably fetch transcripts for a big batch of videos.
- **[Channel → Latest Videos → AI Summaries → Notion](./channel-latest-videos-to-notion.md)** — Auto-summarize a channel's newest videos into Notion.
- **[Channel → Pinecone RAG Ingestion](./channel-pinecone-rag-ingestion.md)** — Ingest a whole channel into a Pinecone vector database.
- **[Channel Sentiment Pipeline](./channel-sentiment-pipeline.md)** — Score a channel's latest videos for sentiment and topics.
- **[Channel → Full Video Metadata DB](./channel-video-metadata-db.md)** — Build a clean database of a channel's videos.
- **[Competitor Analysis Agent](./competitor-analysis-agent.md)** — Get a competitor-intelligence report from their YouTube.
- **[Conference / Talk Archiver](./conference-talk-archiver.md)** — Build a searchable archive of a conference channel's talks.
- **[Multi-Video Content Calendar Auditor](./content-calendar-auditor.md)** — Audit a channel's uploads for themes, gaps, and opportunities.
- **[Course Glossary Builder](./course-glossary-builder.md)** — Build a study glossary from a course playlist.
- **[Customer Testimonial Mining](./customer-testimonial-mining.md)** — Mine ready-to-use customer quotes across a channel's videos.
- **[Discourse Analysis Pipeline](./discourse-analysis-pipeline.md)** — Analyze how a topic is talked about across YouTube.
- **[Earnings-Call Tracker → Analyst Summary → Slack](./earnings-call-tracker.md)** — Get an analyst summary when a company posts an earnings call.
- **[Executive Interview Tracker](./executive-interview-tracker.md)** — Track what an executive is saying publicly, in one entry.
- **[Influencer Vetting Pipeline](./influencer-vetting-pipeline.md)** — Vet a creator for brand-safety and fit before you partner.
- **[Video → Transcript → Key Quotes (with Timestamps)](./key-quotes-extractor.md)** — Pull the most quotable moments, with timestamps.
- **[Lecture Chapter Summary with Timestamps](./lecture-chapter-summary.md)** — Turn a long lecture into a timestamped chapter study guide.
- **[Longitudinal Content Tracker](./longitudinal-content-tracker.md)** — Track how a channel's messaging changes week over week.
- **[Market Commentary Daily Brief](./market-commentary-daily-brief.md)** — Get a daily market brief from finance YouTube channels.
- **[Meeting Prep Research Pack](./meeting-prep-research-pack.md)** — Walk into any meeting prepared, from the company's own videos.
- **[Multi-Channel Content Database Ingester](./multi-channel-content-db.md)** — Build one content database from many channels.
- **[Multi-Video Topic Researcher → Research Brief](./multi-video-topic-researcher.md)** — Research a topic across YouTube and get one tidy brief.
- **[New Course Video → Auto Study Notes → Notion](./new-course-video-notes.md)** — Turn new course videos into study notes in Notion.
- **[New-Upload Monitor → Key Points → Slack Digest](./new-upload-monitor-slack-digest.md)** — Get a Slack digest of key points whenever a channel posts.
- **[New-Upload → Webhook Trigger](./new-upload-webhook-trigger.md)** — Fire a webhook the moment a channel posts a new video.
- **[Niche Channel Discovery Tool](./niche-channel-discovery.md)** — Find the channels that own a niche, ranked.
- **[Onboarding Video → Internal Wiki Page](./onboarding-video-wiki.md)** — Turn an onboarding video into a clean internal wiki page.
- **[Playlist → Full Text Corpus (CSV / JSON)](./playlist-full-text-corpus.md)** — Turn a playlist into a full-text dataset (CSV/JSON).
- **[Playlist → Quiz / Flashcard Generator](./playlist-quiz-flashcards.md)** — Turn a lecture playlist into quizzes and flashcards.
- **[Playlist → Study Notes Generator](./playlist-study-notes.md)** — Turn a course playlist into per-video study notes.
- **[Playlist / Channel → Transcripts → RAG Chunks](./playlist-to-rag-chunks.md)** — Turn a playlist or channel into clean text chunks for AI search.
- **[Policy Speech Monitor](./policy-speech-monitor.md)** — Get a neutral Slack briefing whenever a policy channel posts.
- **[Product Launch Watcher](./product-launch-watcher.md)** — Get a Slack alert the moment a competitor posts a launch.
- **[Share-of-Voice Analyzer](./share-of-voice-analyzer.md)** — Measure how much of a topic your brand owns vs rivals.
- **[Single Video → Transcript → AI Summary](./single-video-transcript-summary.md)** — Turn one YouTube video into a clean transcript and a 5-bullet summary.
- **[Slack Auto-Summary of Shared YouTube Links](./slack-youtube-link-summary.md)** — Auto-summarize YouTube links shared in Slack.
- **[Speaker Research Brief](./speaker-research-brief.md)** — Prep for a guest or keynote with a one-page speaker brief.
- **[Startup Pitch Research Tool](./startup-pitch-research.md)** — Diligence a startup from what its founders say on camera.
- **[Student Q&A → Video Timestamp Lookup](./student-qa-timestamp-lookup.md)** — Answer a question and point to the exact video moment.
- **[Support FAQ Builder from Tutorials](./support-faq-builder.md)** — Draft a support FAQ from your tutorial channel.
- **[Topic Trend Tracker](./topic-trend-tracker.md)** — Track a topic's momentum on YouTube over time.
- **[Transcript Availability Checker](./transcript-availability-checker.md)** — Check which videos actually have captions before a big job.
- **[Video → Short-Form Script Pack](./video-short-form-script-pack.md)** — Turn one long video into 3–5 short-form scripts.
- **[Video → Transcript → Content Classifier](./video-to-content-classifier.md)** — Auto-tag a video with category, topics, and audience.
- **[Video-to-Embedding Batch Processor](./video-to-embedding-batch.md)** — Turn a batch of videos into vector embeddings.
- **[Video → Transcript → LinkedIn Post](./video-to-linkedin-post.md)** — Turn a video into a ready-to-post LinkedIn update.
- **[Video → Transcript → Newsletter Section](./video-to-newsletter-section.md)** — Turn a video into a polished newsletter section.
- **[Video → Transcript → Podcast Show Notes + Chapters](./video-to-podcast-show-notes.md)** — Generate show notes and chapters for any episode.
- **[Single Video → Transcript → SEO Blog-Post Draft](./video-to-seo-blog-post.md)** — Turn any video into a publish-ready SEO blog post.
- **[Video → Transcript → X (Twitter) Thread](./video-to-x-thread.md)** — Turn a video into a ready-to-post 10-tweet X thread.
- **[Video → Transcript → Translate → Multilingual Summary](./video-translate-multilingual-summary.md)** — Get any video's gist, translated into the language you choose.
- **[Weekly Channel Digest → Ghost/Substack Draft](./weekly-channel-digest.md)** — Turn a channel's week into a ready-to-publish newsletter draft.
- **[Weekly Industry Brief across Channels](./weekly-industry-brief.md)** — Get one weekly executive brief across many channels.
- **[YouTube Citation Builder](./youtube-citation-builder.md)** — Generate APA, MLA, Chicago, and BibTeX citations for a video.
- **[YouTube Fact-Check Assistant](./youtube-fact-check-assistant.md)** — Turn a video into a claim-by-claim review sheet for a human checker.
- **[YouTube SEO Title & Description Analyzer](./youtube-seo-analyzer.md)** — See what's ranking for a keyword and how to beat it.

---

Free & open source (MIT). Prefer no-code? Every skill also ships as an [n8n workflow](../n8n).
