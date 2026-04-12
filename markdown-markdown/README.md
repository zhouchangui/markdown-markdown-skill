# markdown-markdown

Skill repository for agents that need to operate the `markdown-markdown` Markdown review CLI.

This skill is for compact Markdown review handoffs.

## Use Cases

- Review one Markdown file or a whole directory.
- Send a clickable global link in IM or Feishu-style chat.
- Keep long-running runs visible with output and status files.

## Dependencies

- `markdown-markdown` npm package
- `cloudflared` only when a public/global URL is required
- A local browser only when running in a local session

## Skills.sh Publishing

This skill is published in a public Git repository and can be installed with `skills.sh` from the repository URL.

Example:

```bash
npx skills add https://github.com/zhouchangui/markdown-markdown-skill
```

Repository: [https://github.com/zhouchangui/markdown-markdown-skill](https://github.com/zhouchangui/markdown-markdown-skill)

## Repository Layout

- `SKILL.md` - required skill entrypoint
- `README.md` - human-facing overview and publishing notes

## Maintainer

- zcg

## License

MIT
