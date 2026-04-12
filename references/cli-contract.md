# CLI Contract

This skill targets the `markdown-markdown` CLI.

## Input

- Accepts a single Markdown file or a directory of Markdown files.
- Path arguments are resolved from the current working directory.

## Modes

- `--browser system` opens the system default browser.
- `--cloudflare` requires a public tunnel and should fail fast if `cloudflared` is unavailable.
- `--no-cloudflare` forces localhost-only operation.

## Output

- The CLI prints the final review JSON to stdout.
- `--output <file>` writes the same JSON to disk.
- `--status-file <file>` writes lifecycle snapshots for orchestration.

## Lifecycle File

- `phase: running` means the review session is active.
- `phase: completed` means the user submitted the review successfully.
- `phase: failed` means the CLI stopped with an error.

## Agent Handoff

- Read `prompt` for the compact edit brief.
- Read `annotations` for the precise user-selected targets.
- Use short anchors and line ranges instead of pasted paragraphs when translating the review into edit instructions.
