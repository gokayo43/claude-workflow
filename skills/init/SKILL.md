---
name: init
description: Set up rules and statusline that plugins can't auto-load. Safe to re-run after updates.
disable-model-invocation: true
model: sonnet
allowed-tools: Bash, Read
---

## Task: Initialize dev-workflow plugin

The plugin auto-loads agents, skills, and commands. But **rules** and **statusline** need to be copied to `~/.claude/` manually. This skill handles that — after verifying required dependencies are installed.

Plugin source directory: ${CLAUDE_SKILL_DIR}/../../

### Step 1: Check dependencies

Check that each required tool is available. For each one, run `which <tool>` or `command -v <tool>`.

Required tools:
- **jq** — JSON parsing (used by statusline and agents). Install: `winget install jqlang.jq` (Windows) / `brew install jq` (macOS) / `sudo apt install jq` (Linux)
- **curl** — API calls for rate-limit display in statusline. Install: usually pre-installed; `winget install curl` (Windows) / `brew install curl` (macOS) / `sudo apt install curl` (Linux)
- **git** — branch/dirty detection in statusline. Install: `winget install Git.Git` (Windows) / `brew install git` (macOS) / `sudo apt install git` (Linux)
- **awk** — number formatting in statusline. Usually pre-installed with Git Bash / shell.

**Important:** On Windows/Git Bash, also check the winget jq path (`$LOCALAPPDATA/Microsoft/WinGet/Links/jq.exe`) since winget installs outside Git Bash's default PATH.

For each tool, report:
- `✓ <tool>` if found (include the path)
- `✗ <tool> — not found. Install with: <install command>` if missing

If any required tool is missing, print the full list of missing tools with install instructions and **stop here** — do not continue to the next steps. Tell the user to install them and re-run `/init`.

### Step 2: Compare and copy rules

For each `.md` file in `${CLAUDE_SKILL_DIR}/../../rules/`:
1. Check if `~/.claude/rules/<filename>` exists
2. If it exists, diff the two files
3. If different or missing, copy the plugin version over
4. Report what was copied, what was already up to date, and what changed

### Step 3: Compare and copy statusline

1. Check if `~/.claude/statusline.sh` exists
2. Diff against `${CLAUDE_SKILL_DIR}/../../statusline/statusline.sh`
3. If different or missing, copy the plugin version over
4. Report result

### Step 4: Check settings.json statusline config

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

### Step 5: Version marker

Write the current plugin version to `~/.claude/.dev-workflow-version`.
Read the version from `${CLAUDE_SKILL_DIR}/../../.claude-plugin/plugin.json`.

### Step 6: Summary

Print a summary:
```
dev-workflow init complete (v<version>)

Dependencies:
  ✓ jq (/usr/bin/jq)
  ✓ curl (/usr/bin/curl)
  ✓ git (/usr/bin/git)
  ✓ awk (/usr/bin/awk)

Rules:
  - comments.md: [copied / up to date / updated]
  - no-complecting.md: [copied / up to date / updated]
  - post-plan-review.md: [copied / up to date / updated]

Statusline:
  - statusline.sh: [copied / up to date / updated]
  - settings.json: [configured / already configured]
```
