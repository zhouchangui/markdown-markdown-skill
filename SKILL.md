---
name: markdown-markdown-skill
description: Use when an agent must collect review on Markdown files through `markdown-markdown review`, especially in local browser sessions or chat-tool handoff flows.
---

# markdown-markdown-skill

Install:
- `npm i markdown-markdown`
- Use `npx markdown-markdown` if the binary is not already on PATH
- `cloudflared` is required for public chat-channel links

Rules:
- Use only the async commands: `review create`, `review wait`, `review refresh`, and `review close`
- Never use the legacy one-shot mode for agent workflows
- Keep only one active review session at a time
- `review create` starts the session and prints startup JSON with `sessionId`, `reviewUrl`, `controlUrl`, `publicUrl`, `round`, and `phase`
- `review wait` is the source of truth for the user's decision
- Chat tools such as Feishu, WeChat, Slack, or Telegram must use `npx markdown-markdown review create --cloudflare <path>`
- Non-chat sessions default to `npx markdown-markdown review create --no-cloudflare --browser system <path>`
- In chat-tool flows, send the returned `reviewUrl` back as a clickable link or card and ask the user to click it
- If a chat-tool flow cannot produce a public link, stop and report the blocker instead of falling back to localhost
- Keep `review wait` listening while the user reviews; if it may take a while, leave it running in a background terminal or separate session
- `finish_review` ends the round
- `continue_review` means apply the payload, run `review refresh`, then wait again
- `abandoned` means stop and ask whether to restart review or continue without it
- Use `rootPath`, `files[*].absolutePath`, `annotations[*].selection`, `selectedText`, and `note` to locate edits
- Do not claim completion until the payload has been read and handled
