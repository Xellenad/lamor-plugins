---
name: commit-message
description: Use when the user asks to write, improve or review a git commit message, or is about to commit changes. Produces a concise Conventional Commits style message.
---

# Commit Message

Write commit messages in the Conventional Commits format.

## Format

```
<type>(<scope>): <short summary in the imperative mood>

<optional body: WHAT and WHY, not HOW>
```

## Types

- `feat` - new functionality
- `fix` - bug fix
- `docs` - documentation only
- `refactor` - code change without behavior change
- `test` - tests
- `chore` - build, dependencies, configs

## Rules

1. Subject line up to 72 characters, no trailing period.
2. Look at the changes first (`git diff --staged`), then write the message.
3. If the changes are unrelated, suggest splitting them into several commits.

## Example

```
feat(auth): add password reset via email

Users could not recover access without contacting support.
```
