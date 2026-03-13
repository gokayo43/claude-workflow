---
model: haiku
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(git add:*), Bash(git commit:*), Bash(git push:*), Bash(git branch:*)
description: Commit and push changes using Haiku
---

## Context

- Current branch: !`git branch --show-current`
- Current git status: !`git status`
- Staged changes: !`git diff --cached`
- Unstaged changes: !`git diff`
- Recent commits for style reference: !`git log --oneline -5`

## Your task

Based on the changes shown above:

1. Stage all relevant changes (use `git add .` or be selective based on the changes)
2. Create a commit with a clear, concise message that:
   - Summarizes the nature of the changes (feature, fix, refactor, etc.)
   - Focuses on the "why" rather than the "what"
   - Follows the commit style from recent commits
3. Push the changes to the remote

If $ARGUMENTS is provided, use it as guidance for the commit message: $ARGUMENTS
