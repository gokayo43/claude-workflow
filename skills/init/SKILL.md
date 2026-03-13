---
name: init
description: Set up rules and statusline that plugins can't auto-load. Safe to re-run after updates.
disable-model-invocation: true
model: haiku
allowed-tools: Bash, Read
---

## Task: Sync plugin files that can't be auto-loaded

The plugin auto-loads agents, skills, and commands. But **rules** and **statusline** need to be copied to `~/.claude/` manually. This skill handles that.

Plugin source directory: ${CLAUDE_SKILL_DIR}/../../

### Step 1: Compare and copy rules

For each `.md` file in `${CLAUDE_SKILL_DIR}/../../rules/`:
1. Check if `~/.claude/rules/<filename>` exists
2. If it exists, diff the two files
3. If different or missing, copy the plugin version over
4. Report what was copied, what was already up to date, and what changed

### Step 2: Compare and copy statusline

1. Check if `~/.claude/statusline.sh` exists
2. Diff against `${CLAUDE_SKILL_DIR}/../../statusline/statusline.sh`
3. If different or missing, copy the plugin version over
4. Report result

### Step 3: Check settings.json statusline config

1. Read `~/.claude/settings.json`
2. Check if `statusLine` is configured
3. If missing, add:
   ```json
   "statusLine": {
     "type": "command",
     "command": "bash ~/.claude/statusline.sh"
   }
   ```
4. If already present, report "already configured"

### Step 4: Version marker

Write the current plugin version to `~/.claude/.dev-workflow-version`.
Read the version from `${CLAUDE_SKILL_DIR}/../../.claude-plugin/plugin.json`.

### Step 5: Summary

Print a summary:
```
dev-workflow init complete (v1.1.0)

Rules:
  - comments.md: [copied / up to date / updated]
  - no-complecting.md: [copied / up to date / updated]
  - post-plan-review.md: [copied / up to date / updated]

Statusline:
  - statusline.sh: [copied / up to date / updated]
  - settings.json: [configured / already configured]
```
