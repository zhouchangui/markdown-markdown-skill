---
name: markdown-markdown-skill
description: Use when an agent needs to review Markdown files with the `markdown-markdown` npm package, especially for local browser review, IM/Feishu chat handoff, or long-running jobs that need output and status files.
---

# markdown-markdown-skill

Concise guidance for agents using the `markdown-markdown` npm package.

## Use Cases

- Review one Markdown file or a whole directory.
- Share a clickable result in IM or Feishu-style chat channels.
- Capture long-running reviews to disk for later automation.

## Dependencies

- `markdown-markdown` npm package must be available.
- `cloudflared` is required only when a public or global URL is needed.
- The system browser is optional; the app window remains the fallback.

## Environment Rule

- IM or Feishu chat: prefer `--cloudflare`, then send the generated URL as a card or link.
- Local terminal session: prefer `--browser system` and open locally.
- If the environment is unclear, ask once, then keep that choice for the run.

## Quick Reference

| Situation | Use |
|-----------|-----|
| Local single file | `npx markdown-markdown --no-cloudflare --browser system ./docs/spec.md` |
| IM or chat handoff | `npx markdown-markdown --cloudflare ./docs/spec.md` |
| Directory review | `npx markdown-markdown --no-cloudflare ./docs` |
| Save final JSON | `--output ./out/review.json` |
| Async completion | `--status-file ./out/review.status.json` |

## Handoff

- `prompt` is the compact edit brief.
- `annotations` are short anchors for locating changes.
- `phase: completed` or `phase: failed` in the status file signals the end of the run.

## Gotchas

- If `--cloudflare` is requested and `cloudflared` is missing, the CLI should fail immediately with an install hint.
- `--status-file` is for orchestration; do not rely on terminal text alone for completion detection.
- Keep annotations short and location-focused.
- Use `--output` plus `--status-file` for long jobs; do not rely on terminal text alone.
