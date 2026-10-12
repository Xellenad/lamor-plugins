---
name: commit-message
description: Use when the user asks to write, improve or review a git commit message, or is about to commit changes. Produces a concise Conventional Commits style message.
---

# Commit Message

Пиши commit-сообщения в формате Conventional Commits.

## Формат

```
<type>(<scope>): <краткое описание в повелительном наклонении>

<необязательное тело: ЧТО и ПОЧЕМУ, а не КАК>
```

## Типы

- `feat` — новая функциональность
- `fix` — исправление бага
- `docs` — только документация
- `refactor` — изменение кода без изменения поведения
- `test` — тесты
- `chore` — сборка, зависимости, конфиги

## Правила

1. Заголовок — до 72 символов, без точки в конце.
2. Сначала посмотри изменения (`git diff --staged`), потом пиши сообщение.
3. Если изменения несвязанные — предложи разбить на несколько коммитов.

## Пример

```
feat(auth): add password reset via email

Users could not recover access without contacting support.
```
