---
name: markdown-markdown-skill
description: Use when an agent must review a Markdown file or directory with the `markdown-markdown` npm package and wait for a structured review payload before editing, especially in local terminal sessions or IM/Feishu handoff flows.
---

# markdown-markdown-skill

Requires:
- `markdown-markdown` installed
- `cloudflared` only when a public URL is required

Environment:
- Local terminal: prefer `--browser system`
- IM/Feishu/chat: prefer `--cloudflare`
- If the environment is unclear, ask once: local browser or public link

Command rule:
- If `markdown-markdown review --help` succeeds, use async session commands
- Otherwise use the legacy one-shot command
- Async mode allows only one active review session at a time

Async flow:
1. `markdown-markdown review create <path>`
2. `markdown-markdown review wait`
3. `finish_review`: apply the payload and continue normal work
4. `continue_review`: apply the payload, run `markdown-markdown review refresh`, then `markdown-markdown review wait` again
5. `abandoned`: stop and ask whether to restart review or continue without review
6. `markdown-markdown review close` if cleanup is still needed

Legacy flow:
1. `markdown-markdown [--browser system|--cloudflare|--no-cloudflare] <path>`
2. Wait for the returned JSON payload
3. Treat the startup URL as setup only, not completion

Payload rules:
- The returned JSON is the source of truth
- Use `rootPath`, `files[*].absolutePath`, `annotations[*].selection`, `selectedText`, and `note` to locate edits
- Do not claim completion until the payload has been read and handled
