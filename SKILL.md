---
name: markdown-markdown-skill
description: Use when an agent needs a human-in-the-loop Markdown review SOP through `npx markdown-markdown review` for a file or directory.
---

# markdown-markdown-skill

Use this skill when an agent has produced a longer Markdown file that should be reviewed by a human before continuing. It is especially useful in chat-tool environments, where a dedicated review page communicates proposed changes more clearly than a long back-and-forth in the chat box.

## When to use

- A user asks for review on one Markdown file or a directory of Markdown files.
- The workflow needs an async review loop instead of the legacy one-shot flow.
- The current conversation is in a chat tool and needs a public review link.
- The review should open in a local browser session instead of chat.

## SOP

1. Determine the target.
   Use one target path only: either a single Markdown file or a directory that contains Markdown files.
2. Determine the environment.
   If the current conversation is in Feishu, WeChat, Slack, Telegram, or another chat tool that needs a clickable public link, use the chat-tool flow. Otherwise use the local flow.
3. Create the review session.
   Chat-tool flow: `npx markdown-markdown review create --cloudflare <file-or-directory>`
   Local flow: `npx markdown-markdown review create --no-cloudflare --browser system <file-or-directory>`
   If chat-tool `--cloudflare` creation fails, retry once after checking the error message.
   If the retry still fails and the user needs a public link, stop and report the blocker instead of silently falling back to localhost.
   Only switch from chat-tool flow to local flow when the agent knows the user can use the same machine's browser for review.
4. Read the startup JSON from `review create`.
   Capture `sessionId`, `reviewUrl`, `controlUrl`, `publicUrl`, `round`, and `phase`.
   In chat-tool flow, treat a missing `publicUrl` or a failed create command as a failed public-link setup.
5. Hand the user into the review UI.
   In chat tools, send `reviewUrl` back as a clickable link or card and ask the user to open it.
   In local flow, let the system browser open the page.
6. Wait for the review result.
   Run `npx markdown-markdown review wait` and treat its returned JSON as the source of truth.
   After starting `review wait`, keep monitoring that process until it exits. Do not assume the user will remind you to read the result.
7. Branch on the returned action.
   If the action is `continue_review`, edit the files from the returned payload, then run `npx markdown-markdown review refresh`, then wait again.
   If the action is `finish_review`, apply any last requested edits if needed, close the session with `npx markdown-markdown review close`, then continue the rest of the task.
   If the session is `abandoned`, stop the review flow, close the session, and report the blocker or ask whether to restart review.
   Treat `abandonedReason` or `reason` as the best explanation of what happened, for example `browser_closed_by_user`.
8. Repeat until review is finished.
   The loop is `wait -> inspect payload -> edit files -> refresh -> wait` until the user finishes review.

## Applying review results

- Prefer the returned JSON over your own interpretation of the page state.
- Use `prompt` as the compact change summary.
- Use `rootPath`, `files[*].absolutePath`, `annotations[*].selection`, `selectedText`, and `note` to find the exact edit locations.
- For a single-file review, update only that file unless the payload clearly points elsewhere.
- For a directory review, apply edits file by file and preserve unaffected Markdown files.

## Rules

- Use only the async commands: `review create`, `review wait`, `review refresh`, and `review close`.
- Never use the legacy one-shot flow for agent work.
- Keep only one active review session at a time.
- Treat `review wait` as the source of truth for the user's decision.
- Always invoke the CLI as `npx markdown-markdown ...`.
- `review wait` is not fire-and-forget. Keep it in the foreground, or if you move it to another session, proactively poll that session until it exits.
- On chat-tool `--cloudflare` failure, retry once, then surface the blocker. Do not silently fall back to localhost when a public link is required.
- Local fallback is allowed only when the agent knows the user can review in the local browser on the same machine.
- Do not call `review refresh` before handling a `continue_review` payload.
- On `abandoned`, read and report `abandonedReason` or `reason` before deciding whether to restart.
- Close the session after `finish_review` or `abandoned` before moving on.
- Do not claim completion until the returned payload has been read and handled.

## Reference

See [references/cli-contract.md](references/cli-contract.md) for the exact command shapes, file-vs-directory notes, and payload fields.
