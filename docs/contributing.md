# Contributing

How to add, change and test plugins in `lamor-plugins`. The whole catalog is one file: `.claude-plugin/marketplace.json`. Every plugin is one entry in its `plugins` array.

## Adding an external plugin

Most plugins here point to an upstream repository. Pick the source type by where the plugin lives and whether it has its own manifest.

### 1. Plugin at the root of a GitHub repo, with its own `.claude-plugin/plugin.json`

Use a `github` source. Example: `chrome-devtools-mcp`, `superpowers`, `modern-web-guidance`.

```json
{
  "name": "superpowers",
  "description": "Skills for brainstorming, planning, subagent-driven development, systematic debugging and TDD",
  "author": { "name": "Jesse Vincent" },
  "homepage": "https://github.com/obra/superpowers",
  "category": "development",
  "source": { "source": "github", "repo": "obra/superpowers" }
}
```

### 2. Plugin in a subdirectory of a repo (monorepo)

Use a `git-subdir` source. Example: `context7`, and every plugin taken from `anthropics/claude-plugins-official`.

```json
{
  "name": "commit-commands",
  "description": "Commands for git commit workflows including commit, push, and PR creation",
  "author": { "name": "Anthropic" },
  "homepage": "https://github.com/anthropics/claude-plugins-official/tree/main/plugins/commit-commands",
  "category": "productivity",
  "source": {
    "source": "git-subdir",
    "url": "anthropics/claude-plugins-official",
    "path": "plugins/commit-commands"
  }
}
```

### 3. Configuration lives in the upstream marketplace entry (`strict: false`)

Some plugins, like the LSP plugins in `claude-plugins-official`, have an almost empty folder: their config (`lspServers`, `mcpServers`...) is written inside the upstream `marketplace.json` entry with `"strict": false`. Pointing to the folder is not enough, copy the config block into our entry too. Example: `typescript-lsp`.

```json
{
  "name": "typescript-lsp",
  "source": {
    "source": "git-subdir",
    "url": "anthropics/claude-plugins-official",
    "path": "plugins/typescript-lsp"
  },
  "strict": false,
  "lspServers": {
    "typescript": {
      "command": "typescript-language-server",
      "args": ["--stdio"],
      "extensionToLanguage": { ".ts": "typescript", ".tsx": "typescriptreact" }
    }
  }
}
```

### 4. Repo is not a Claude plugin (no `plugin.json`), only skills

When the upstream repo has no `plugin.json`, our entry becomes the manifest. Declare the components it should load. Example: `angular-skills`, where `"skills": ["./"]` scans the repo root for `<name>/SKILL.md` folders.

```json
{
  "name": "angular-skills",
  "source": { "source": "github", "repo": "angular/skills" },
  "skills": ["./"]
}
```

### How to tell which case applies

Look at the upstream repository:

1. Is there `.claude-plugin/marketplace.json`? Find the plugin's entry: its `source` shows the folder, and `strict: false` with inline config means case 3.
2. Is there `.claude-plugin/plugin.json` in the plugin folder? Then case 1 or 2.
3. Only skill folders with `SKILL.md`? Case 4.

If the plugin is listed in [claude-plugins-official](https://github.com/anthropics/claude-plugins-official/blob/main/.claude-plugin/marketplace.json), copy its entry as the starting point and turn a relative `source` (like `./plugins/foo`) into a `git-subdir` source.

### Pinning a version

By default an external plugin follows the default branch of the upstream repo. To pin it, add `ref` (branch or tag) or `sha` (exact commit) to the `source`:

```json
"source": { "source": "github", "repo": "obra/superpowers", "sha": "5bf4e78011075bcfc0dc295f0724994cd123ee71" }
```

Pinning gives stability, but you have to bump the `sha` yourself to get updates.

## Adding a local plugin

For a plugin whose code lives in this repository:

1. Create `plugins/<name>/` with `.claude-plugin/plugin.json` (name, version, description, author).
2. Add components:

```
plugins/<name>/
  .claude-plugin/plugin.json   <- manifest
  skills/<skill>/SKILL.md      <- skills and slash commands
  agents/<agent>.md            <- subagents
  hooks/hooks.json             <- hooks
  .mcp.json                    <- MCP servers
```

3. Add an entry with `"source": "./plugins/<name>"` to `marketplace.json`.
4. Add a `README.md` in the plugin folder.

`plugins/hello-toolkit` is a working example of every component type: a skill, a slash command, a subagent and a hook. It is kept as a template and is not listed in the catalog. To try it, load it directly: `claude --plugin-dir ./plugins/hello-toolkit`.

## Checklist for every change

Every catalog change must ship with its documentation. This rule is also in [CLAUDE.md](../CLAUDE.md), so Claude Code follows it automatically in this repository.

1. Edit `.claude-plugin/marketplace.json`.
2. Add a row to the plugin table in `README.md` and a section to `docs/plugins.md` (what it does, source, requirements, how to use).
3. Validate:

```bash
claude plugin validate .                      # the catalog
claude plugin validate plugins/<name>         # a local plugin
```

The validator checks the format only. A wrong `repo` or `path` shows up only at install time, so test the install (see below).

4. Commit and push. Users get the change after `/plugin marketplace update lamor-plugins`.

## Testing locally

Add your working copy as a marketplace, without pushing:

```
/plugin marketplace add ./path/to/lamor-plugins
/plugin install <plugin>@lamor-plugins
```

For local plugins, edits are picked up at the next session start or with `/reload-plugins`. Check `/plugin` -> **Errors** if something does not load.

If you already added the GitHub version of `lamor-plugins`, remove it first (`claude plugin marketplace remove lamor-plugins`), because both use the same name.

## Conventions

- Plugin `name`: kebab-case, the same as upstream when possible.
- Write descriptions in English, one sentence.
- Add `author` and `homepage` for external plugins, so it is clear where the code comes from.
- `category`: `development`, `productivity`, `security`, `documentation` or `learning`.
