# Openclaw-config

> by [ara.so](https://ara.so) — 1.5K+ installs on [skills.sh](https://skills.sh/adisinghstudent/ara.so/openclaw-config)

Agent skill for managing, debugging, and operating [OpenClaw](https://github.com/Aradotso/zeroclaw) — the open-source AI agent runtime with 30+ LLM providers and 14 messaging channels.

Install it and your coding agent instantly knows how to diagnose issues, search sessions, edit config, and troubleshoot every channel.

## Install

```bash
npx skills add adisinghstudent/ara.so
```

Works with Claude Code, Cursor, Codex, Windsurf, Cline, GitHub Copilot, and [30+ other agents](https://skills.sh/adisinghstudent/ara.so/openclaw-config).

## Repository Structure

```
.
├── README.md
└── skills/
    └── openclaw-config/
        └── SKILL.md          # 850 lines of operational knowledge
```

## What's in the Skill

### Diagnostics

- **Quick health check** — one-shot command that checks gateway, config JSON, channels, plugins, credentials, cron, recent errors, and memory DB
- **Known error patterns** — 12 documented errors with exact meaning and fix

### File Map

Complete reference for everything in `~/.openclaw/`:

```
~/.openclaw/
├── openclaw.json                    # Main config — channels, auth, gateway, plugins
├── agents/main/
│   ├── agent/auth-profiles.json     # LLM auth tokens
│   └── sessions/
│       ├── sessions.json            # Session index
│       └── *.jsonl                  # Session transcripts
├── workspace/                       # Agent workspace
│   ├── SOUL.md                      # Personality and tone
│   ├── IDENTITY.md                  # Name and branding
│   ├── USER.md                      # Owner context
│   ├── AGENTS.md                    # Operating rules
│   ├── BOOT.md                      # Startup instructions
│   ├── HEARTBEAT.md                 # Periodic task checklist
│   ├── MEMORY.md                    # Long-term curated memory
│   ├── TOOLS.md                     # Contacts, SSH hosts
│   ├── memory/                      # Daily logs
│   └── skills/                      # Workspace-level skills
├── memory/main.sqlite               # Vector memory DB (Gemini embeddings)
├── logs/
│   ├── gateway.log                  # Runtime events
│   └── gateway.err.log              # Errors
├── cron/
│   ├── jobs.json                    # Job definitions
│   └── runs/                        # Per-job run logs
├── credentials/
│   ├── whatsapp/default/            # Baileys session (~1400 files)
│   ├── telegram/{bot}/token.txt     # Bot tokens
│   └── bird/cookies.json            # X/Twitter auth
├── extensions/{name}/               # Custom plugins (TypeScript)
├── browser/openclaw/user-data/      # Chromium profile
└── tools/signal-cli/                # Signal CLI binary
```

### Channel Troubleshooting

Deep troubleshooting playbooks for each channel:

| Channel | Covered Issues |
|---------|---------------|
| **WhatsApp** | No reply to messages, 408 timeouts, cross-context blocks, session lookup, allowlist/group policy, lane congestion, full disconnection, credential wipe |
| **Telegram** | Config validation errors (`botToken` vs `token`), polling timeouts, offset stuck, bot "forgetting" (compaction), correct config template |
| **Signal** | RPC failures, signal-cli process health, rate limiting, wrong target format, profile name spam, daemon restart |
| **Cron** | Job status overview, failure details, run log parsing, common failure causes, next scheduled times, disabling broken jobs |

### Memory System

Three-layer memory architecture:

1. **Context window** — in-session, pruned by compaction
2. **Workspace files** — `MEMORY.md` + daily logs in `memory/`
3. **Vector DB** — SQLite + Gemini embeddings with FTS5 search

Includes commands for inspecting each layer, checking embedding rate limits, and rebuilding the index.

### Session Search

- Find conversations by person, channel, or date
- Search message content across all sessions
- Read specific session transcripts with formatted output
- Understand the JSONL format (session, message, compaction, model_change events)

### Config Editing

Safe edit patterns using `jq`:
- Switch channel policies (open, allowlist, pairing, disabled)
- Enable autopilot mode
- Change LLM model
- Set concurrency limits
- Enable/disable plugins
- Backup and restore

### Security Modes

| Mode | Behavior | Risk |
|------|----------|------|
| `open` + `allowFrom: ["*"]` | Anyone can message, bot responds to all | HIGH |
| `allowlist` + `allowFrom: ["+1..."]` | Only listed numbers | LOW |
| `pairing` | Unknown senders get approval code | LOW |
| `disabled` | Channel off | NONE |

### Extending OpenClaw

- **Skills** — markdown knowledge packs via ClawdHub or `npx skills add`
- **Extensions** — custom TypeScript channel plugins
- **Cron jobs** — scheduled autonomous tasks
- **Multi-agent** — spawn Codex/Claude Code/Pi as background workers
- **Cross-channel** — receive on WhatsApp, notify on Signal
- **Canvas** — push HTML/dashboards to connected devices
- **Voice calls** — Twilio/Telnyx/Plivo integration

## Example Usage

Ask your agent any of these:

```
Why is my WhatsApp channel not connecting?
Show me the last 10 sessions on Telegram
Search all sessions for "deployment"
Switch Signal to allowlist mode for just my number
Which cron jobs are failing and why?
How do I add a new Telegram bot?
```

The skill gives the agent the exact commands, files, and fixes — no guessing.

## Links

- [skills.sh](https://skills.sh/adisinghstudent/ara.so/openclaw-config)
- [OpenClaw (ZeroClaw)](https://github.com/Aradotso/zeroclaw)
- [ara.so](https://ara.so) — instant AI agent environments in the cloud
- [Trending Skills](https://github.com/Aradotso/trending-skills) — auto-generated skills from GitHub trending

## License

MIT
