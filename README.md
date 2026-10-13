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
