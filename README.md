# dev-workflow

Claude Code plugin with review agents, commit commands, and coding rules.

## Install

```bash
/plugin marketplace add gokayo43/claude-workflow
/plugin install dev-workflow@claude-workflow
```

## What's included

- **Agents**: 3 parallel review agents (ripple-tracer, correctness-auditor, architecture-data-flow-auditor)
- **Skills**: `/dev-workflow:commit`, `/dev-workflow:commit-my-changes` (inline, uses haiku, keeps conversation context)
- **Commands**: `/deep-review`
- **Rules**: comment policy, no-complecting, post-plan-review

## Statusline (manual)

Copy `statusline/statusline.sh` to `~/.claude/statusline.sh`, then add to `~/.claude/settings.json`:

```json
{
  "statusLine": {
    "type": "command",
    "command": "bash ~/.claude/statusline.sh"
  }
}
```
