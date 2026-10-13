# gitlab-workflow

A bundle for projects hosted on gitlab.com: the GitLab MCP server plus commit and merge request commands built on the `glab` CLI.

| Component | What it does |
|---|---|
| dependency `gitlab` | GitLab MCP server: merge requests, pipelines, issues, wikis |
| `/gitlab-workflow:commit` | Commits the current changes with a generated message |
| `/gitlab-workflow:commit-push-mr [target]` | Commits, creates a branch if you are on the default one, pushes and opens a merge request |

Short forms `/commit` and `/commit-push-mr` work when no other installed plugin has a command with the same name.

Requirements: the [`glab`](https://gitlab.com/gitlab-org/cli) CLI, authenticated with `glab auth login`. The GitLab MCP server asks you to sign in to gitlab.com on first use.

Install for a project:

```bash
claude plugin install gitlab-workflow@lamor-plugins --scope project
```
