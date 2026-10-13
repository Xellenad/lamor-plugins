# Plugin guide

How to use every plugin in `lamor-plugins`. Install any of them with:

```
/plugin install <plugin>@lamor-plugins
```

How plugin components are triggered:

- Skills: Claude loads them on its own when the task matches the skill's description. You can also call one explicitly as `/<plugin>:<skill>`.
- Commands: you run them, as `/<command>` or with the full name `/<plugin>:<command>` when another command has the same name.
- Agents: Claude starts them as subagents in a separate context; you can also ask for one by name ("use the code-explorer agent to ...").
- Hooks: run automatically on events (session start, file edit, end of turn).
- MCP and LSP servers: add tools that Claude calls when needed.

Contents:

- Documentation and language tooling: [context7](#context7), [typescript-lsp](#typescript-lsp)
- Frontend: [angular-skills](#angular-skills), [modern-web-guidance](#modern-web-guidance), [chrome-devtools-mcp](#chrome-devtools-mcp)
- Development workflow: [feature-dev](#feature-dev), [superpowers](#superpowers)
- Review and git: [code-review](#code-review), [pr-review-toolkit](#pr-review-toolkit), [commit-commands](#commit-commands)
- Safety and automation: [security-guidance](#security-guidance), [hookify](#hookify)

---

## context7

Up-to-date, version-specific documentation and code examples for libraries and frameworks, pulled into Claude's context on demand.

- Source: [upstash/context7](https://github.com/upstash/context7) (Upstash)
- Type: MCP server (hosted remotely at `mcp.context7.com`, nothing runs locally)
- Requirements: internet access

Usage: just ask about a library. Claude resolves the library ID and fetches the current docs before answering.

```
How do I set up httpResource in Angular 21?
Show the current Prisma syntax for nested writes. use context7
```

Adding "use context7" to a prompt forces a lookup. Useful for fast-moving libraries where the model's training data may be outdated.

## typescript-lsp

Connects the TypeScript language server, so Claude gets real code intelligence: go to definition, find references, diagnostics and type errors right after edits.

- Source: [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/typescript-lsp) (Anthropic)
- Type: LSP server
- Files: `.ts`, `.tsx`, `.js`, `.jsx`, `.mts`, `.cts`, `.mjs`, `.cjs`
- Requirements: install the server globally, it must be on `PATH`:

```bash
npm install -g typescript-language-server typescript
```

Usage: nothing to call. Claude uses it automatically when navigating and editing TypeScript/JavaScript code, and sees type errors immediately after a change.

## angular-skills

Official Angular team skills for writing modern, idiomatic Angular code.

- Source: [angular/skills](https://github.com/angular/skills) (Angular Team, generated from `angular/angular`)
- Type: skills
- Requirements: Node.js and npm for `angular-new-app`

| Skill | What it does |
|---|---|
| `angular-developer` | Code generation and architectural guidance: components, services, signals, `linkedSignal`, `resource`/`httpResource`, forms, DI, routing, SSR, accessibility, animations, styling, testing, CLI. Loads detailed references only when needed |
| `angular-new-app` | Creates a new Angular application with the Angular CLI following current best practices |

Usage: triggered automatically by Angular tasks, or explicitly:

```
/angular-skills:angular-new-app
Create a standalone component with a signal-based form for user registration
```

## modern-web-guidance

Keeps Claude current with modern web platform features and best practices (HTML, CSS, JavaScript APIs, performance, accessibility).

- Source: [GoogleChrome/modern-web-guidance](https://github.com/GoogleChrome/modern-web-guidance) (Google Chrome)
- Type: skills: `modern-web-guidance`, `chrome-extensions`

Usage: triggered automatically on frontend work. Ask explicitly when choosing an approach:

```
What is the modern way to build a dialog without a library?
```

## chrome-devtools-mcp

Lets Claude control and inspect a live Chrome browser: open pages, click and fill forms, read console messages with source-mapped stack traces, analyze network requests, record performance traces, take screenshots.

- Source: [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) (Chrome DevTools Team)
- Type: MCP server (started locally via `npx`) plus skills: `chrome-devtools`, `chrome-devtools-cli`, `a11y-debugging`, `cookie-debugging`, `debug-optimize-lcp`, `memory-leak-debugging`, `troubleshooting`
- Requirements: Node.js, Google Chrome. Under WSL, Chrome must be available inside Linux.

Usage:

```
Open http://localhost:4200, log in as a test user and check the console for errors
Record a performance trace of the home page and explain why LCP is slow
```

## feature-dev

A structured workflow for building a feature: understand the codebase, ask clarifying questions, design the architecture, implement, review.

- Source: [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/feature-dev) (Anthropic)
- Command: `/feature-dev <description>`
- Agents: `code-explorer` (traces existing code), `code-architect` (designs the implementation), `code-reviewer` (checks quality)

Usage:

```
/feature-dev Add pagination to the orders table with server-side sorting
```

Best for features that touch several files. For a one-line fix it is overkill.

## superpowers

A library of process skills that make Claude work in a disciplined way: brainstorm before building, write a plan, do test-driven development, debug systematically, verify before claiming success.

- Source: [obra/superpowers](https://github.com/obra/superpowers) (Jesse Vincent)
- Type: skills plus a `SessionStart` hook that teaches Claude to use them

Main skills:

| Skill | When it applies |
|---|---|
| `brainstorming` | Before creating features or changing behavior |
| `writing-plans`, `executing-plans` | Multi-step tasks: write a plan, then execute it |
| `subagent-driven-development`, `dispatching-parallel-agents` | Delegating independent tasks to subagents |
| `test-driven-development` | Writing code test-first |
| `systematic-debugging` | Any bug or failing test |
| `verification-before-completion` | Before saying "done" |
| `requesting-code-review`, `receiving-code-review` | Review loops |
| `using-git-worktrees`, `finishing-a-development-branch` | Branch isolation and integration |
| `writing-skills` | Authoring your own skills |

Usage: skills trigger automatically. Call one explicitly when you want it, for example `/superpowers:brainstorming`.

## code-review

Automated pull request review by several specialized agents, with confidence scoring to filter out false positives.

- Source: [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/code-review) (Anthropic)
- Command: `/code-review:code-review` (run on a branch with an open PR, or pass the PR number)
- Requirements: a GitHub repository with an open pull request and the `gh` CLI, authenticated. It takes project `CLAUDE.md` rules into account and can post the review as a PR comment

Note: Claude Code also has a built-in `/code-review` command. Use the full name `/code-review:code-review` to run the plugin version.

## pr-review-toolkit

A set of focused review agents, each checking one aspect of a change.

- Source: [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/pr-review-toolkit) (Anthropic)
- Command: `/review-pr [aspects]` (runs the relevant agents on the current changes or PR; optionally name the aspects to check)

| Agent | Checks |
|---|---|
| `code-reviewer` | General quality and project guidelines |
| `code-simplifier` | Unnecessary complexity |
| `comment-analyzer` | Accuracy and usefulness of comments |
| `pr-test-analyzer` | Test coverage gaps |
| `silent-failure-hunter` | Swallowed errors, bad fallbacks |
| `type-design-analyzer` | Type design and invariants |

Usage:

```
/review-pr
Use silent-failure-hunter on the changes in src/api
```

## commit-commands

Shortcuts for everyday git work.

- Source: [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/commit-commands) (Anthropic)
- Requirements: git; `gh` CLI for creating PRs

| Command | What it does |
|---|---|
| `/commit` | Creates a commit with a generated message from the current changes |
| `/commit-push-pr` | Commits, pushes the branch and opens a pull request |
| `/clean_gone` | Deletes local branches whose remote branch is gone |

## security-guidance

Security review of code Claude writes, in three layers:

1. Pattern warnings on every edit (hardcoded secrets, `innerHTML`, unsafe deserialization and ~25 more patterns).
2. LLM review of the diff at the end of each turn; high-severity findings go back to Claude to fix.
3. Agentic review on `git commit` that traces data flow across files (IDOR, auth bypass, SSRF).

- Source: [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/security-guidance) (Anthropic)
- Type: hooks, works automatically
- Requirements: Python 3.8+ on `PATH`

Cost: layers 2 and 3 make extra model calls. Tune them with environment variables:

| Variable | Effect |
|---|---|
| `SECURITY_GUIDANCE_DISABLE=1` | Disable the whole plugin |
| `ENABLE_STOP_REVIEW=0` | Disable the end-of-turn diff review |
| `ENABLE_COMMIT_REVIEW=0` | Disable the commit review |
| `ENABLE_PATTERN_RULES=0` | Disable pattern warnings |
| `SECURITY_REVIEW_MODEL=<model id>` | Use a different (for example cheaper) model for the diff review |

## hookify

Create hooks that block or warn about unwanted behavior, described as simple markdown rules instead of hand-written hook scripts.

- Source: [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/hookify) (Anthropic)
- Requirements: Python 3 on `PATH`

| Command | What it does |
|---|---|
| `/hookify <instruction>` | Creates a rule, for example `/hookify warn me before any rm -rf` |
| `/hookify` (no arguments) | Analyzes the conversation and suggests rules for mistakes it found |
| `/hookify:list` | Lists rules |
| `/hookify:configure` | Enables or disables rules |
| `/hookify:help` | Help |

Rules are stored as `.claude/hookify.<rule>.local.md` files in your project. Edit a file to change a rule, set `enabled: false` to turn it off, or delete the file to remove it.
