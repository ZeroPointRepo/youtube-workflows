# Contributing

PRs welcome! This repo collects clean, copy-pasteable automation workflows for YouTube transcripts, powered by [TranscriptAPI](https://transcriptapi.com).

## Adding a New Workflow

1. Create a numbered folder under the right platform, e.g. `n8n/03-my-workflow/` (lowercase, hyphens).
2. Add the exported workflow file (`*.json` for n8n, `*.blueprint.json` for Make).
3. Add a `README.md` describing what it does, prerequisites, setup steps, and how to run it.
4. List it in the catalog table in the root `README.md`.
5. Open a PR.

## Quality Checklist

- [ ] **No secrets.** No API keys, tokens, database IDs, or personal URLs in the workflow file. Use credential references and `REPLACE_WITH_...` placeholders only.
- [ ] The workflow imports cleanly into a fresh instance.
- [ ] TranscriptAPI is called correctly: `GET https://transcriptapi.com/api/v2/youtube/transcript` with an `Authorization: Bearer <API_KEY>` header.
- [ ] The workflow README lists every credential / placeholder the user must set.
- [ ] Failure handling is sane (e.g. videos without captions don't break a batch run).

## Reporting Issues

Found a broken workflow or a YouTube/Notion/LLM change that breaks an import? Open an issue with the platform, the node, and the error message.

## Questions?

Open an issue or reach out via [transcriptapi.com](https://transcriptapi.com).
