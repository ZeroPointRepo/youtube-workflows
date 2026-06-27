# 54 · YouTube Citation Builder

Cite a video properly in one run. Paste a URL and it transcribes (with metadata) and generates citation-ready references in **APA, MLA, Chicago, and BibTeX**, plus a few timestamped snippet quotes with deep-links and in-text citations you can drop into a paper or notes. No LLM — it's deterministic and costs a single transcript credit.

```
Start → Set Video URL → Get Transcript (TranscriptAPI) → Build Citation Fields → Format Citations
```

## What each node does

| Node | Type | Purpose |
|------|------|---------|
| **Start** | Manual Trigger | Click **Test workflow** to run. |
| **Set Video URL** | Edit Fields | The video to cite (`videoUrl`). |
| **Get Transcript (TranscriptAPI)** | HTTP Request | `GET /api/v2/youtube/transcript` (`send_metadata=true`) for title + channel (author). |
| **Build Citation Fields** | Code | Pulls author/title/URL and picks a few evenly-spaced snippet segments. |
| **Format Citations** | Code | Assembles `citations: { apa, mla, chicago, bibtex }`, an `accessed` date, and `snippets[]` with deep-links + in-text citations. |

## Output shape

```jsonc
{
  "videoUrl": "...",
  "author": "Channel name",
  "title": "Video title",
  "citations": { "apa": "...", "mla": "...", "chicago": "...", "bibtex": "@misc{...}" },
  "accessed": "YYYY-MM-DD",
  "snippets": [ { "timestamp": "1:23", "seconds": 83, "quote": "...", "deepLink": "...&t=83s", "intext_citation": "(Author, 1:23)" } ],
  "caveat": "..."
}
```

## Prerequisites

- An [n8n](https://n8n.io) instance.
- A **TranscriptAPI** key — [free, 100 credits](https://transcriptapi.com).

## Setup

1. **Import** `youtube-citation-builder.json`.
2. Create one **Header Auth** credential and attach it to **Get Transcript**:
   | Credential | Header name | Header value |
   |------------|-------------|--------------|
   | `TranscriptAPI - Authorization Bearer` | `Authorization` | `Bearer YOUR_TRANSCRIPTAPI_KEY` |
3. Open **Set Video URL** and paste your video URL.
4. Click **Test workflow**. **Format Citations** holds the references.

## Customizing

- **More quotes:** raise the snippet count in **Build Citation Fields** (default picks 4 evenly-spaced segments).
- **Your style only:** keep just the field you need (`citations.apa`, etc.) in **Format Citations**.
- **Export:** add a Docs/Notion node to append the citation + snippets to a research doc.

## Cost

One run = **1 TranscriptAPI credit** (only if the video has a transcript). No LLM cost.

## Notes

- **Citation templates — verify before submitting.** The API does **not** provide a publication date, so citations use **`n.d.`** (no date); the **author is the channel name**. Confirm both, and confirm the exact style edition your context requires (APA 7 / MLA 9 / Chicago 17) and any institution/publisher rules. The `accessed` date is set at run time.
