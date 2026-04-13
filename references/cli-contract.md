# markdown-markdown review CLI contract

Dependencies:
- `markdown-markdown`
- `cloudflared` for public chat-channel links

Target:
- `npx markdown-markdown review create <file-or-directory>` accepts either one Markdown file or one directory.
- In directory mode, the CLI discovers Markdown files under that target and serves their local image assets through the review host.

Environment:
- Chat tools such as Feishu, WeChat, Slack, or Telegram: `npx markdown-markdown review create --cloudflare <path>` because the user needs a public link to continue the conversation
- Non-chat sessions: `npx markdown-markdown review create --no-cloudflare --browser system <path>`

Cloudflare fallback:
- `--cloudflare` is the hard-requirement path for chat-tool review links.
- If `review create --cloudflare` fails, retry once after inspecting the error.
- If the retry still fails, treat that as a blocker for chat-tool review and surface the error clearly.
- Do not silently switch to localhost when the user still needs a public link.
- Switch to `--no-cloudflare --browser system` only when the agent knows the user can complete review in the local browser on the same machine.

Session rules:
- One active review session at a time
- Use only the async commands: `review create`, `review wait`, `review refresh`, and `review close`
- Never use the legacy one-shot mode for agent review workflows
- `review create` starts the session and prints `sessionId`, `reviewUrl`, `controlUrl`, `publicUrl`, `round`, and `phase`
- In chat-tool flow, a missing `publicUrl` means the public-link setup did not succeed
- `review wait` returns the review result and is the source of truth
- `review wait` must stay actively monitored until it exits; if the agent runs it in another terminal or background session, it must proactively poll or reattach instead of waiting for the user to nudge it
- Chat-tool flows must send the returned `reviewUrl` back as a clickable hyperlink and tell the user to click it for review
- If the chat channel supports cards, rich messages, or buttons, prefer sending the review link inside a card instead of plain text
- If a chat-tool flow cannot create a public URL, stop and surface that blocker
- Keep `review wait` listening while the user is reviewing; move it to a background terminal or separate session if the wait is long

Loop:
1. `npx markdown-markdown review create ...`
2. Start or keep `npx markdown-markdown review wait` listening
3. `continue_review` -> read payload -> edit files -> `npx markdown-markdown review refresh` -> wait again
4. `finish_review` -> read payload -> apply any final edits if needed -> `npx markdown-markdown review close`
5. `abandoned` -> read `abandonedReason` or `reason` -> `npx markdown-markdown review close` -> report or recover

Payload rule:
- Use the returned JSON, not the startup metadata, as the handoff artifact
- `prompt` is the compact instruction summary for the agent
- `files[*].absolutePath` identifies which files to edit
- `annotations[*].selection`, `selectedText`, and `note` help locate and understand each requested change
