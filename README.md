# markdown-markdown-skill

Skill for agents that use the async `markdown-markdown review` CLI to review Markdown files.

## Install

```bash
npx skills add https://github.com/zhouchangui/markdown-markdown-skill
npm i markdown-markdown
```

## Rules

- Prefer `--browser system` in local terminal sessions.
- Prefer `--cloudflare` in IM or Feishu handoff flows.
- `review create` is setup only; `review wait` returns the review result.
- If the review result is `continue_review`, apply edits, run `review refresh`, then wait again.
- If the review result is `abandoned`, stop and ask whether to restart review or continue without it.

## Files

- `SKILL.md` - concise execution contract
- `references/cli-contract.md` - compact command notes
- `LICENSE` - MIT license
