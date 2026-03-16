# openclaw-config

> by [ara.so](https://ara.so) — 1.5K installs on [skills.sh](https://skills.sh/adisinghstudent/ara.so/openclaw-config)

Agent skill for managing, debugging, and operating [OpenClaw](https://github.com/Aradotso/zeroclaw) — the open-source AI agent runtime. Gives your coding agent full knowledge of OpenClaw's config, logs, sessions, and internals.

## Install

```bash
npx skills add adisinghstudent/ara.so
```

Works with Claude Code, Cursor, Codex, Windsurf, Cline, GitHub Copilot, and [30+ other agents](https://skills.sh/adisinghstudent/ara.so/openclaw-config).

## What it does

Once installed, your AI agent can:

- **Diagnose issues** — health checks, gateway status, process monitoring
- **Search sessions** — find conversations by keyword, contact, channel, or date
- **Analyze logs** — filter gateway events, errors, channel-specific logs
- **Debug cron jobs** — failed jobs, run history, next scheduled times
- **Inspect memory** — SQLite queries against the persistent memory DB
- **Check channels** — WhatsApp, Telegram, Signal, iMessage, Twitter status and policies
- **Verify credentials** — health checks for all channel auth configs
- **Edit config safely** — `jq` one-liners for model, concurrency, and policy changes
- **Troubleshoot** — playbooks for common failures (disconnects, RPC errors, broken config)

## Example

Ask your agent:

```
Why is my WhatsApp channel not connecting?
```

The skill gives the agent the exact commands to run, files to check, and fixes to apply.

## Links

- [skills.sh page](https://skills.sh/adisinghstudent/ara.so/openclaw-config)
- [OpenClaw](https://github.com/Aradotso/zeroclaw)
- [ara.so](https://ara.so)

## License

MIT
