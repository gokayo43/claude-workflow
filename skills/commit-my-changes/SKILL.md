---
name: commit-my-changes
description: Commit only changes made by Claude, skip files with mixed authorship
disable-model-invocation: true
model: haiku
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(git add:*), Bash(git commit:*), Bash(git push:*), Bash(git branch:*), Bash(git stash:*)
---

## Context

- Current branch: !`git branch --show-current`
- Current git status: !`git status`
- Staged changes: !`git diff --cached`
- Unstaged changes: !`git diff`
- Recent commits for style reference: !`git log --oneline -5`

## Your task

You must commit ONLY files whose changes were made by Claude in the current session. Follow this process strictly:

### Step 1: Identify Claude's changes

Review the conversation history above this command invocation. Identify every file that Claude (you, in the prior turns) created, wrote to, or edited using the Write, Edit, or Bash tools. Build an explicit list of these files.

### Step 2: Identify all changed files

From `git status`, list every file with modifications (staged or unstaged) or that is untracked.

### Step 3: Classify each changed file

For each changed file from Step 2, classify it:

- **Claude-only**: The file appears in Claude's edit list (Step 1) AND every hunk in `git diff` for that file corresponds to changes Claude made. **Include this file.**
- **Mixed**: The file appears in Claude's edit list BUT `git diff` also shows hunks/changes that Claude did NOT make (pre-existing uncommitted changes from the user). **Exclude this file.**
- **Not Claude's**: The file does NOT appear in Claude's edit list at all. **Exclude this file.**

### Step 4: Report classification

Before committing, print a summary:

```
Files to commit (Claude-only):
  - path/to/file1.js
  - path/to/file2.ts

Files skipped (mixed changes - contains both Claude and non-Claude edits):
  - path/to/file3.css

Files skipped (not Claude's changes):
  - path/to/file4.js
```

If there are zero files to commit, say so and stop.

### Step 5: Stage and commit

1. Stage ONLY the Claude-only files by name (never use `git add .` or `git add -A`)
2. Create a commit with a clear, concise message that:
   - Summarizes the nature of the changes (feature, fix, refactor, etc.)
   - Focuses on the "why" rather than the "what"
   - Follows the commit style from recent commits
3. Push the changes to the remote

If $ARGUMENTS is provided, use it as guidance for the commit message: $ARGUMENTS
