---
name: commit
description: Create a git commit from the current changes with a generated message.
disable-model-invocation: true
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Recent commits: !`git log --oneline -10`

## Your task

Based on the above changes, create a single git commit.

1. Stage the relevant files with `git add`. Do not stage files that look like secrets (`.env`, credentials, keys).
2. Write the message in the style of the recent commits (for example Conventional Commits if the project uses them): a short subject line, and a body only if the change needs explaining.
3. Create the commit.

Do not push. Do not send any other text besides the tool calls and a one-line summary of the commit.
