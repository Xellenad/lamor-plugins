# lamor-plugins

A curated plugin marketplace for [Claude Code](https://claude.com/claude-code): language tooling, documentation lookup, code review, git workflows, browser debugging, Angular and web best practices, security checks and development workflow skills, all installable from one place.

All plugins here are external: the catalog points to their upstream repositories, so you always get the authors' code.

## Quick start

In a Claude Code session:

```
/plugin marketplace add Xellenad/lamor-plugins
/plugin install context7@lamor-plugins
```

Or from your shell, without starting a session:

```bash
claude plugin marketplace add Xellenad/lamor-plugins
claude plugin install context7@lamor-plugins
```

Run `/reload-plugins` (or start a new session) to load what you installed. Browse everything in the catalog with `/plugin` -> **Discover** tab.

## Plugins

| Plugin | What it gives you | Requirements |
|---|---|---|
| [context7](docs/plugins.md#context7) | Up-to-date library documentation via MCP | - |
| [typescript-lsp](docs/plugins.md#typescript-lsp) | TypeScript/JavaScript code intelligence (LSP) | `typescript-language-server`, `typescript` |
| [angular-skills](docs/plugins.md#angular-skills) | Official Angular skills: code generation, new apps | Node.js, npm for new apps |
| [modern-web-guidance](docs/plugins.md#modern-web-guidance) | Current web platform best practices | - |
| [chrome-devtools-mcp](docs/plugins.md#chrome-devtools-mcp) | Control and debug a live Chrome browser | Node.js, Chrome |
| [feature-dev](docs/plugins.md#feature-dev) | Guided feature development with agents | - |
| [code-review](docs/plugins.md#code-review) | Multi-agent pull request review | `gh` CLI for PRs |
| [pr-review-toolkit](docs/plugins.md#pr-review-toolkit) | Specialized review agents (tests, errors, types...) | - |
| [commit-commands](docs/plugins.md#commit-commands) | Commit, push and PR commands | git, `gh` CLI for PRs |
| [security-guidance](docs/plugins.md#security-guidance) | Security warnings and automatic diff review | Python 3.8+ |
| [superpowers](docs/plugins.md#superpowers) | Brainstorming, planning, TDD, debugging workflows | - |
| [hookify](docs/plugins.md#hookify) | Create hooks from simple markdown rules | Python 3 |
| [github-workflow](docs/plugins.md#github-workflow) | Bundle for GitHub projects: MCP, commit/PR commands, PR review | `gh` CLI, GitHub token |
| [gitlab-workflow](docs/plugins.md#gitlab-workflow) | Bundle for gitlab.com projects: MCP, commit/MR commands | `glab` CLI |
| [github](docs/plugins.md#github) | GitHub MCP server (part of `github-workflow`) | GitHub token |
| [gitlab](docs/plugins.md#gitlab) | GitLab MCP server (part of `gitlab-workflow`) | gitlab.com account |

The full guide for each plugin, with commands and usage examples, is in [docs/plugins.md](docs/plugins.md).

## Choose your git platform

Pick one bundle per project, depending on where the project is hosted. Installing a bundle installs everything it depends on.

| Project on | Install | You get |
|---|---|---|
| GitHub | `claude plugin install github-workflow@lamor-plugins --scope project` | GitHub MCP, `/commit`, `/commit-push-pr`, `/code-review:code-review`, `/review-pr` |
| gitlab.com | `claude plugin install gitlab-workflow@lamor-plugins --scope project` | GitLab MCP, `/gitlab-workflow:commit`, `/gitlab-workflow:commit-push-mr` |

With `--scope project` the choice is saved in the project's `.claude/settings.json`. Commit that file and every teammate gets the same bundle for that project, while other projects can use the other one.

Do not install both bundles in the same project: the GitHub commands call `gh` and will fail on a GitLab remote, and the other way round.

## Managing plugins

Every action is available in two places: interactively through `/plugin` in a session, or as a `claude plugin ...` command in your shell (handy for scripts and new machines).

### Install

```
/plugin install <plugin>@lamor-plugins
```

```bash
claude plugin install <plugin>@lamor-plugins                  # for you, in every project (user scope)
claude plugin install <plugin>@lamor-plugins --scope project  # for everyone in this repository
claude plugin install <plugin>@lamor-plugins --scope local    # for you, in this repository only
```

Scopes:

| Scope | Applies to | Stored in |
|---|---|---|
| `user` (default) | you, all projects | `~/.claude/settings.json` |
| `project` | everyone working in the repo | `.claude/settings.json` (commit it) |
| `local` | you, this repo only | `.claude/settings.local.json` |

### Update

This marketplace is third-party, so auto-update is off by default. Update manually:

```
/plugin marketplace update lamor-plugins
```

In the `/plugin` -> **Marketplaces** tab, select `lamor-plugins` -> **Update marketplace** to refresh the catalog and update every plugin you installed from it in one go. To update a single plugin:

```bash
claude plugin update <plugin>@lamor-plugins
```

Note: `claude plugin marketplace update` in the shell only refreshes the catalog listing, it does not update installed plugins.

To turn on auto-update: `/plugin` -> **Marketplaces** -> `lamor-plugins` -> **Enable auto-update**. Updates are applied at the next session start, or after `/reload-plugins`.

### Disable, enable, uninstall

```bash
claude plugin disable <plugin>@lamor-plugins     # keep installed, stop loading
claude plugin enable <plugin>@lamor-plugins
claude plugin uninstall <plugin>@lamor-plugins
```

In a session, use the **Installed** tab in `/plugin`.

### See what is installed

```bash
claude plugin list               # installed plugins, version, scope, status
claude plugin details <plugin>   # components and per-session token cost
claude plugin marketplace list   # registered marketplaces
```

### Remove the marketplace

```bash
claude plugin marketplace remove lamor-plugins
```

This also uninstalls every plugin you installed from it.

## Set up for a team

To give everyone in a project the same plugins, commit `.claude/settings.json` in that project:

```json
{
  "extraKnownMarketplaces": {
    "lamor-plugins": {
      "source": { "source": "github", "repo": "Xellenad/lamor-plugins" }
    }
  },
  "enabledPlugins": {
    "context7@lamor-plugins": true,
    "typescript-lsp@lamor-plugins": true,
    "angular-skills@lamor-plugins": true
  }
}
```

After trusting the folder, Claude Code offers to add the marketplace. Each teammate installs the enabled plugins once with `claude plugin install <plugin>@lamor-plugins --scope project`.

## Avoiding duplicates

Several plugins here also exist in other marketplaces, for example `claude-plugins-official` (added to Claude Code by default). If you already have one of them installed from there, uninstall it before installing the same plugin from `lamor-plugins`, otherwise both copies load: skills and hooks run twice, MCP servers start twice.

```bash
claude plugin list                                          # check where each plugin comes from
claude plugin uninstall context7@claude-plugins-official    # remove the other copy
claude plugin install context7@lamor-plugins
```

## Troubleshooting

- Plugin installed but nothing changed: run `/reload-plugins` or start a new session.
- Errors after install: open `/plugin` -> **Errors** tab.
- LSP or MCP plugin does not start: check its requirements in the table above (for example, `typescript-language-server` must be on `PATH`).
- Catalog looks outdated: `/plugin marketplace update lamor-plugins`.

## Repository layout

```
.claude-plugin/marketplace.json   <- the catalog: marketplace name, owner, plugin list
plugins/hello-toolkit/            <- example plugin, a template for local plugins (not in the catalog)
docs/plugins.md                   <- guide for every plugin
docs/contributing.md              <- how to add or change plugins
LICENSE                           <- MIT, covers this repository; each external plugin keeps its own license
```

## Contributing

See [docs/contributing.md](docs/contributing.md) for how to add a plugin (local or external), validate the catalog and test changes locally.

## License

[MIT](LICENSE) for the contents of this repository. External plugins are distributed under their own licenses, see their upstream repositories.
