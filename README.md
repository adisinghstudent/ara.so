# ara.so — Agent Skills

Agent skills by [ara.so](https://ara.so). Includes hand-crafted skills and auto-generated daily skills from GitHub's trending open source projects.

Every hour, a GitHub Actions workflow checks what's trending on GitHub and creates a comprehensive agent skill for the top project.

## Install all skills

```bash
npx skills add adisinghstudent/ara.so
```

## Install a specific skill

```bash
npx skills add adisinghstudent/ara.so --skill lightpanda-browser
npx skills add adisinghstudent/ara.so --skill openclaw-config
```

## Skills Index

| Skill | Description | Source | Date |
|-------|-------------|--------|------|
| [openclaw-config](skills/openclaw-config/) | Manage OpenClaw bot configuration — channels, agents, security, autopilot | — | — |
| [lightpanda-browser](skills/lightpanda-browser/) | Headless browser built in Zig for AI and automation | [lightpanda-io/browser](https://github.com/lightpanda-io/browser) | 2026-03-15 |
| [gstack](skills/gstack/) | Use Garry Tan's exact Claude Code setup: 6 opinionated tools that serve as CEO, Eng Manager, Release Manager and QA Engineer | [garrytan/gstack](https://github.com/garrytan/gstack) | 2026-03-15 |
| [pi-autoresearch](skills/pi-autoresearch/) | Autonomous experiment loop extension for pi | [davebcn87/pi-autoresearch](https://github.com/davebcn87/pi-autoresearch) | 2026-03-15 |
<!-- SKILL_INDEX -->

## How daily skills work

1. GitHub Actions cron runs every hour
2. Fetches the hottest new repos (created in the last 7 days, sorted by stars)
3. Skips repos that already have a skill
4. Uses the Claude API to generate a comprehensive SKILL.md from the repo's README
5. Commits, pushes, and registers on skills.sh

## By ara.so

[Ara](https://ara.so) — instant AI agent environments in the cloud.

## License

MIT
