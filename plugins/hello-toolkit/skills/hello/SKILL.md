---
name: hello
description: Greets the user by name and briefly lists what the hello-toolkit plugin contains.
argument-hint: <имя>
disable-model-invocation: true
---

# Hello

Пользователь вызвал команду с аргументами: $ARGUMENTS

1. Поприветствуй пользователя по имени из аргументов (если имени нет — просто «Привет!»).
2. Коротко перечисли, что есть в плагине hello-toolkit:
   - skill `commit-message` — помогает писать commit-сообщения;
   - команда `/hello-toolkit:hello` — эта команда;
   - субагент `code-explainer` — объясняет код простым языком;
   - hook `SessionStart` — приветствие при старте сессии.
