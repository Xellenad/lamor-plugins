# lamor-plugins

A Claude Code plugin marketplace. The catalog is `.claude-plugin/marketplace.json`; almost all plugins are external entries pointing to upstream repositories.

## Rule: every change to the catalog is documented

Whenever a plugin is added, changed or removed, update all of these in the same change:

1. `.claude-plugin/marketplace.json` - the entry itself (`name`, `description`, `author`, `homepage`, `category`, `source`).
2. `README.md` - the row in the Plugins table: name linking to `docs/plugins.md#<name>`, what it gives, requirements.
3. `docs/plugins.md` - a section for the plugin: what it does, source (repo and author), type (skills, commands, agents, hooks, MCP, LSP), requirements, how to use with example prompts or commands, notes on cost or conflicts. Also add it to the contents list at the top.
4. `docs/contributing.md` - only if the change introduces a new kind of source or a new pattern not yet described there.

Then run `claude plugin validate .` and check that README anchors match headings in `docs/plugins.md`.

## How to add an external plugin

Inspect the upstream repo first, then pick the source type (details and examples in `docs/contributing.md`):

- plugin at repo root with `.claude-plugin/plugin.json` -> `"source": {"source": "github", "repo": "owner/repo"}`
- plugin in a subfolder -> `"source": {"source": "git-subdir", "url": "owner/repo", "path": "..."}`
- upstream entry has `strict: false` with inline config (LSP plugins) -> copy that config into our entry with `strict: false`
- no `plugin.json`, only skill folders -> our entry is the manifest, declare components (e.g. `"skills": ["./"]`)

Verify commands, skills, agents and requirements against the upstream sources before writing docs; do not guess.

## Conventions

- All repository content is in English.
- Commit and push only when the user asks.
