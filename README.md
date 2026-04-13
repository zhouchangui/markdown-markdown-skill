# markdown-markdown-skill

Skill for agents that use the async `markdown-markdown review` CLI to review Markdown files.

## Install

Install the skill:

```bash
npx skills add https://github.com/zhouchangui/markdown-markdown-skill
```

Install the CLI tool:

```bash
npm i markdown-markdown
```

The review startup JSON includes `sessionId`, `reviewUrl`, `controlUrl`, `publicUrl`, `round`, and `phase`. Agents should treat `review wait` as the source of truth for the user's decision.

## Rules

- Always use the async review commands: `review create`, `review wait`, `review refresh`, and `review close`.
- Never use the legacy one-shot `markdown-markdown <path>` flow for agent work.
- Chat-tool channels such as Feishu, WeChat, Slack, or Telegram must use `--cloudflare`.
- After a chat-tool `review create`, read the returned `reviewUrl`, send it back to the user as a clickable hyperlink, and tell the user to click it to do the review.
- If the chat channel supports cards, rich messages, or buttons, prefer sending the review link as a card for a better user experience.
- If a chat-tool flow cannot produce a public link, stop and surface the blocker instead of falling back to localhost.
- Non-chat channels default to `--no-cloudflare --browser system`, which opens the localhost page in the system browser.
- `review create` is setup only; `review wait` returns the review result.
- Keep `review wait` listening while the user is reviewing. If the wait is long, keep that listener in a background terminal or separate session and continue other work.
- If the review result is `continue_review`, apply edits, run `review refresh`, then wait again.
- If the review result is `abandoned`, stop and ask whether to restart review or continue without it.

## Channel Playbook

- Chat tool flow:
  1. `npx markdown-markdown review create --cloudflare <path>`
  2. Read `reviewUrl` from the startup JSON
  3. Prefer a clickable card or button when the channel supports it; otherwise send a message like `[点击这里进行 review](https://...)`
  4. Keep `npx markdown-markdown review wait` listening until the session resolves
- Local flow:
  1. `npx markdown-markdown review create --no-cloudflare --browser system <path>`
  2. Let the system browser open the localhost page
  3. Keep `npx markdown-markdown review wait` listening until the session resolves

## Files

- `SKILL.md` - concise execution contract
- `references/cli-contract.md` - compact command notes
- `LICENSE` - MIT license
