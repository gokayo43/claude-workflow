# dev-workflow

Claude Code plugin for a disciplined dev workflow: parallel code review, smart commits, and coding rules.

## Install

```bash
/plugin marketplace add gokayo43/claude-workflow
/plugin install dev-workflow@claude-workflow
/reload-plugins
/dev-workflow:init
```

The `init` skill copies rules and statusline to `~/.claude/` (plugins can't auto-load those). Safe to re-run after updates.

## What's included

### Auto-loaded by plugin
- **Agents**: 3 parallel review agents (ripple-tracer, correctness-auditor, architecture-data-flow-auditor) — all use Opus
- **Skills**: `/dev-workflow:commit`, `/dev-workflow:commit-my-changes` — run inline with Haiku, no subagent overhead, full conversation context
- **Commands**: `/dev-workflow:deep-review` — launches all 3 review agents in parallel

### Synced via `/dev-workflow:init`
- **Rules**: comment policy, no-complecting, post-plan-review
- **Statusline**: session info, context usage, rate limits, git branch

## Updating

```bash
/plugin marketplace update claude-workflow
/reload-plugins
/dev-workflow:init
```
