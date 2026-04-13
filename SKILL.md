---
name: markdown-markdown-skill
description: Use when an agent must review a Markdown file or directory with the `markdown-markdown` review CLI, especially in local terminal sessions or IM/Feishu handoff flows.
---

# markdown-markdown-skill

Requires:
- `markdown-markdown` installed
- `cloudflared` only for public links

Environment:
- Local terminal: `markdown-markdown review create --no-cloudflare --browser system <path>`
- IM/Feishu/chat: `markdown-markdown review create --cloudflare <path>`
- If the environment is unclear, ask once: local browser or public link

Rules:
- Use only one active review session at a time
- `review create` starts the session; it does not return the review result
- The review result comes from `review wait`

Flow:
1. `markdown-markdown review create <path>`
2. `markdown-markdown review wait`
3. `finish_review`: apply the payload and continue normal work
4. `continue_review`: apply the payload, run `markdown-markdown review refresh`, then `markdown-markdown review wait` again
5. `abandoned`: stop and ask whether to restart review or continue without review
6. `markdown-markdown review close` if cleanup is still needed

Payload:
- Treat the returned JSON as the source of truth
- Use `rootPath`, `files[*].absolutePath`, `annotations[*].selection`, `selectedText`, and `note` to locate edits
- Do not claim completion until the payload has been read and handled
