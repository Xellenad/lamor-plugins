# hello-toolkit

A learning plugin that contains one component of each type.

| Component | File | Invoked by |
|---|---|---|
| Skill `commit-message` | `skills/commit-message/SKILL.md` | Claude itself, when the task matches the `description` |
| Command `/hello-toolkit:hello` | `skills/hello/SKILL.md` | The user (`disable-model-invocation: true` - Claude does not invoke it on its own) |
| Subagent `code-explainer` | `agents/code-explainer.md` | Claude via the Agent tool, in a separate context |
| Hook `SessionStart` | `hooks/hooks.json` | Claude Code automatically when a session starts |

`.claude-plugin/plugin.json` is the manifest: name, version, description, author.

> Slash commands used to live in `commands/*.md`. That still works, but the new format is `skills/<name>/SKILL.md`: the same file can be both a command and a skill.
