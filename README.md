# lamor-plugins

A learning plugin marketplace for [Claude Code](https://claude.com/claude-code).

## Installation

```
/plugin marketplace add Xellenad/lamor-plugins
/plugin install hello-toolkit@lamor-plugins
```

Update after changes in the repository:

```
/plugin marketplace update lamor-plugins
```

## Plugins

| Plugin | Contents |
|---|---|
| [hello-toolkit](plugins/hello-toolkit) | skill, slash command, subagent, hook |
| [context7](https://github.com/upstash/context7) | MCP server for up-to-date library docs (external, by Upstash) |
| [typescript-lsp](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/typescript-lsp) | TypeScript/JavaScript LSP (external, by Anthropic; needs `npm i -g typescript-language-server typescript`) |
| [code-review](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/code-review) | multi-agent PR code review command (external, by Anthropic) |
| [commit-commands](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/commit-commands) | commit, push and PR creation commands (external, by Anthropic) |
| [chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) | MCP server to control and debug Chrome (external, by Chrome DevTools Team; needs Node.js and Chrome) |
| [angular-skills](https://github.com/angular/skills) | skills `angular-developer` and `angular-new-app` (external, by Angular Team) |
| [feature-dev](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/feature-dev) | feature development workflow with agents (external, by Anthropic) |
| [pr-review-toolkit](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/pr-review-toolkit) | specialized PR review agents (external, by Anthropic) |
| [modern-web-guidance](https://github.com/GoogleChrome/modern-web-guidance) | skills with up-to-date web best practices (external, by Google Chrome) |
| [security-guidance](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/security-guidance) | security warnings and review via hooks (external, by Anthropic) |
| [superpowers](https://github.com/obra/superpowers) | skills for planning, TDD, debugging and subagent workflows (external, by Jesse Vincent) |
| [hookify](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/hookify) | create hooks from markdown rules (external, by Anthropic) |

## Marketplace layout

```
.claude-plugin/marketplace.json   <- catalog: marketplace name, owner, plugin list
plugins/<plugin>/
  .claude-plugin/plugin.json      <- plugin manifest
  skills/<name>/SKILL.md          <- skills and slash commands
  agents/<name>.md                <- subagents
  hooks/hooks.json                <- hooks
  .mcp.json                       <- MCP servers (optional)
```

## Adding a new plugin

1. Create a `plugins/<new-plugin>/` folder with `.claude-plugin/plugin.json`.
2. Add components (skills, agents, hooks...).
3. Add an entry to `plugins` in `.claude-plugin/marketplace.json`.
4. Validate: `claude plugin validate .`
5. Commit and push.

## Local development

```
/plugin marketplace add C:\path\to\lamor-plugins
```
