# markdown-markdown review CLI contract

Dependencies:
- `markdown-markdown`
- `cloudflared` only for public links

Environment:
- Local: `review create --no-cloudflare --browser system <path>`
- IM/Feishu: `review create --cloudflare <path>`

Session rules:
- One active review session at a time
- `review create` starts the session
- `review wait` returns the review result

Loop:
1. `review create <path>`
2. `review wait`
3. `finish_review` -> apply payload, continue
4. `continue_review` -> apply payload, `review refresh`, wait again
5. `abandoned` -> stop and ask the user what to do next
6. `review close` if cleanup is still needed

Payload rule:
- Use the returned JSON, not the startup metadata, as the handoff artifact
