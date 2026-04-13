# markdown-markdown-skill

Skill for agents that use the `markdown-markdown` npm package to review Markdown files.

## Install

```bash
npx skills add https://github.com/zhouchangui/markdown-markdown-skill
npm i markdown-markdown
```

## Rules

- Prefer `--browser system` in local terminal sessions.
- Prefer `--cloudflare` in IM or Feishu handoff flows.
- If `markdown-markdown review --help` succeeds, use the async session flow.
- Otherwise use the legacy one-shot flow and wait for the returned JSON.
- If the review result is `abandoned`, stop and ask whether to restart review or continue without it.

## Files

- `SKILL.md` - concise execution contract
- `references/cli-contract.md` - compact command notes
- `LICENSE` - MIT license
