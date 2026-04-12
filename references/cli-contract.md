# markdown-markdown CLI contract

This skill targets the `markdown-markdown` npm package.

## Required behavior

- Accept a Markdown file or directory.
- Prefer a local browser for local work.
- Prefer a global URL for IM or Feishu chat handoff.
- Write review output to disk when long-running jobs need a file handoff.
- Emit a status file so another agent can detect completion.

## Key flags

- `--browser system` for the OS browser.
- `--cloudflare` when a public URL is required.
- `--no-cloudflare` when local-only access is enough.
- `--output <file>` to write the review JSON.
- `--status-file <file>` to write run state.

## Handoff shape

- `prompt` should stay short and actionable.
- `annotations` should use compact anchors, not long pasted blocks.
- `phase: completed` means the review is ready.
- `phase: failed` means the run should stop and surface the error.
