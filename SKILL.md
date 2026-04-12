---
name: markdown-markdown-agent
description: Use when an agent needs to review Markdown files with markdown-markdown, choose file or directory input, control browser or Cloudflare tunnel behavior, capture final review JSON, or emit lifecycle status for long-running review jobs.
---

# markdown-markdown-agent

Practical guidance for agents that need to run `markdown-markdown` on one file or a directory of Markdown files and turn the result into compact, actionable edit instructions.

## Quick Reference

| Situation | Use |
|-----------|-----|
| Single file review | `npx markdown-markdown --no-cloudflare --browser system ./docs/spec.md` |
| Batch review | `npx markdown-markdown --no-cloudflare ./docs` |
| Public URL required | `npx markdown-markdown --cloudflare ./docs/spec.md` |
| Save final JSON | `--output ./out/review.json` |
| Signal long-running completion | `--status-file ./out/review.status.json` |

## Agent Workflow

1. Pick the smallest useful target. Use a file for one document, a directory for a review batch.
2. Decide the network mode first.
   - Use `--cloudflare` when a public URL is required and missing `cloudflared` should fail fast.
   - Use `--no-cloudflare` when local-only access is enough.
3. Decide the browser mode.
   - Use `--browser system` when you want the OS browser.
   - Leave the default app window when you want the compact controlled shell.
4. For long reviews, always add both `--output` and `--status-file`.
   - Treat `phase: completed` in the status file as the end-of-run signal.
   - Treat `phase: failed` as a hard stop and surface the `error` field.
5. Use the final JSON payload as the handoff artifact.
   - `prompt` is the compact edit brief for another AI.
   - `annotations` are the source of truth for what changed.

## Gotchas

- If `--cloudflare` is set and `cloudflared` is missing, the CLI should fail immediately with an install hint.
- `--status-file` is for orchestration; do not rely on terminal text alone for completion detection.
- Prefer short, precise annotations. The CLI is optimized for compact anchors, not long pasted blocks.
- When the user only wants a browser preview, omit `--output` and `--status-file`.

## Source

- Created for the `markdown-markdown` review CLI in this repository.
