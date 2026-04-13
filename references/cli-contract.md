# markdown-markdown CLI contract

Dependencies:
- `markdown-markdown`
- `cloudflared` only for public links

Environment:
- Local: `--browser system`
- IM/Feishu: `--cloudflare`

Version rule:
- If `markdown-markdown review --help` succeeds, use async session commands
- Otherwise use the legacy one-shot command

Async session loop:
1. `markdown-markdown review create <path>`
2. `markdown-markdown review wait`
3. `finish_review` -> apply payload, continue
4. `continue_review` -> apply payload, `markdown-markdown review refresh`, wait again
5. `abandoned` -> stop and ask the user what to do next

Legacy loop:
1. `markdown-markdown ... <path>`
2. Wait for the returned JSON payload

Payload rule:
- Use the returned JSON, not the startup URL, as the handoff artifact
