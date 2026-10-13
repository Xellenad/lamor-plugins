# github-workflow

A bundle for projects hosted on GitHub. It has no code of its own: installing it installs its dependencies from `lamor-plugins`.

| Dependency | What it adds |
|---|---|
| `github` | GitHub MCP server: issues, pull requests, code search |
| `commit-commands` | `/commit`, `/commit-push-pr`, `/clean_gone` |
| `code-review` | `/code-review:code-review` for pull requests |
| `pr-review-toolkit` | `/review-pr` and specialized review agents |

Requirements: `gh` CLI authenticated (`gh auth login`), and `GITHUB_PERSONAL_ACCESS_TOKEN` in the environment for the GitHub MCP server.

Install for a project:

```bash
claude plugin install github-workflow@lamor-plugins --scope project
```
