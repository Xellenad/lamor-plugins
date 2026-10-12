# hello-toolkit

Учебный плагин, в котором есть по одному компоненту каждого типа.

| Компонент | Файл | Кто вызывает |
|---|---|---|
| Skill `commit-message` | `skills/commit-message/SKILL.md` | Claude сам, когда задача подходит под `description` |
| Команда `/hello-toolkit:hello` | `skills/hello/SKILL.md` | Пользователь (`disable-model-invocation: true` — Claude сам её не вызывает) |
| Субагент `code-explainer` | `agents/code-explainer.md` | Claude через инструмент Agent, отдельный контекст |
| Hook `SessionStart` | `hooks/hooks.json` | Claude Code автоматически при старте сессии |

`.claude-plugin/plugin.json` — манифест: имя, версия, описание, автор.

> Раньше slash-команды клали в `commands/*.md`. Это всё ещё работает, но новый формат —
> `skills/<имя>/SKILL.md`: один и тот же файл может быть и командой, и skill-ом.
