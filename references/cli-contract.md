# markdown-markdown review CLI contract

Dependencies:
- Install the CLI with `npm i markdown-markdown`
- Use `npx markdown-markdown` for one-off runs if needed
- `cloudflared` for public chat-channel links

Environment:
- Chat tools such as Feishu, WeChat, Slack, or Telegram: `npx markdown-markdown review create --cloudflare <path>`
- Non-chat sessions: `npx markdown-markdown review create --no-cloudflare --browser system <path>`

Session rules:
- One active review session at a time
- Use only the async commands: `review create`, `review wait`, `review refresh`, and `review close`
- Never use the legacy one-shot mode for agent review workflows
- `review create` starts the session and prints `sessionId`, `reviewUrl`, `controlUrl`, `publicUrl`, `round`, and `phase`
- `review wait` returns the review result and is the source of truth
- Chat-tool flows must send the returned `reviewUrl` back as a clickable hyperlink and tell the user to click it for review
- If the chat channel supports cards, rich messages, or buttons, prefer sending the review link inside a card instead of plain text
- If a chat-tool flow cannot create a public URL, stop and surface that blocker
- Keep `review wait` listening while the user is reviewing; move it to a background terminal or separate session if the wait is long

Loop:
1. `npx markdown-markdown review create ...`
2. Start or keep `npx markdown-markdown review wait` listening
3. `finish_review` -> apply payload, continue
4. `continue_review` -> apply payload, `npx markdown-markdown review refresh`, wait again
5. `abandoned` -> stop and ask the user what to do next
6. `npx markdown-markdown review close` if cleanup is still needed

Payload rule:
- Use the returned JSON, not the startup metadata, as the handoff artifact
