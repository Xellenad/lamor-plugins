---
name: commit-push-mr
description: Commit the current changes, push the branch and open a GitLab merge request with glab.
argument-hint: "[target branch]"
disable-model-invocation: true
allowed-tools: Bash(git switch:*), Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git push:*), Bash(git symbolic-ref:*), Bash(glab mr create:*)
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Default branch: !`git symbolic-ref --short refs/remotes/origin/HEAD`
- Recent commits: !`git log --oneline -10`

Target branch from arguments (empty means the default branch): $ARGUMENTS

## Your task

1. If the current branch is the default branch, create a new branch with `git switch -c <name>`, where the name describes the change (for example `feat/order-pagination`).
2. Stage the relevant files and create a single commit in the style of the recent commits. Do not stage files that look like secrets.
3. Push the branch: `git push -u origin <branch>`.
4. Create the merge request:

```
glab mr create --title "<title>" --description "<description>" --target-branch <target> --remove-source-branch --yes
```

   - title: the commit subject
   - description: a short summary of what changed and why, plus how to test it if relevant
   - target: the branch from the arguments, or the default branch

5. Reply with the merge request URL that `glab` printed.

If `glab` is not installed or not authenticated, stop after the push and tell the user to run `glab auth login`.
