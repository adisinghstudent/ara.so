# openclaw-config

Full reference for managing, debugging, and searching everything in `~/.openclaw` — channels, sessions, logs, cron jobs, memory, extensions, and credentials.

## Install

```bash
npx skills add adisinghstudent/ara.so
```

## What's Inside

- **Full file map** of every directory and file in `~/.openclaw/`
- **Session search** — find conversations by keyword, contact, channel, or date
- **Log analysis** — gateway events, errors, channel-specific filtering
- **Cron debugging** — failed jobs, run history, next scheduled times
- **Memory inspection** — SQLite queries for the persistent memory DB
- **Channel status** — quick overview of all channel configs and policies
- **Credential health checks** — verify WhatsApp, Telegram, Signal, Twitter creds
- **Config editing** — safe `jq` one-liners for common changes (model, concurrency, policies)
- **Troubleshooting playbooks** for: channel not connecting, Signal RPC failures, cron failures, WhatsApp disconnect, iMessage permissions, broken config, finding old messages

## License

MIT
