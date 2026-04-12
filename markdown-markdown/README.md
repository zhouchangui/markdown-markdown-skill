# markdown-markdown

Skill repository for agents that need to operate the `markdown-markdown` Markdown review CLI.

This skill teaches agents how to:

- choose between file and directory input
- decide when to require Cloudflare, disable it, or stay local
- open the review in the system browser or the compact app window
- write final review JSON to disk
- emit lifecycle status files for asynchronous orchestration
- turn the review output into compact edit instructions for another AI

## Skills.sh Publishing

This skill is published in a public Git repository and can be installed with `skills.sh` from the repository URL.

Example:

```bash
npx skills add https://github.com/zhouchangui/markdown-markdown-agent
```

Repository: [https://github.com/zhouchangui/markdown-markdown-agent](https://github.com/zhouchangui/markdown-markdown-agent)

## Repository Layout

- `SKILL.md` - required skill entrypoint
- `README.md` - human-facing overview and publishing notes

## Maintainer

- zcg

## License

MIT
