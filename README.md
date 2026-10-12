# lamor-plugins

Учебный маркетплейс плагинов для [Claude Code](https://claude.com/claude-code).

## Установка

```
/plugin marketplace add Xellenad/lamor-plugins
/plugin install hello-toolkit@lamor-plugins
```

Обновить после изменений в репозитории:

```
/plugin marketplace update lamor-plugins
```

## Плагины

| Плагин | Что внутри |
|---|---|
| [hello-toolkit](plugins/hello-toolkit) | skill, slash-команда, субагент, hook |

## Как устроен маркетплейс

```
.claude-plugin/marketplace.json   ← каталог: имя маркетплейса, владелец, список плагинов
plugins/<плагин>/
  .claude-plugin/plugin.json      ← манифест плагина
  skills/<имя>/SKILL.md           ← skills и slash-команды
  agents/<имя>.md                 ← субагенты
  hooks/hooks.json                ← hooks
  .mcp.json                       ← MCP-серверы (по желанию)
```

## Как добавить новый плагин

1. Создать папку `plugins/<новый-плагин>/` с `.claude-plugin/plugin.json`.
2. Добавить компоненты (skills, agents, hooks…).
3. Добавить запись в `plugins` в `.claude-plugin/marketplace.json`.
4. Проверить: `claude plugin validate .`
5. Закоммитить и запушить.

## Локальная разработка

```
/plugin marketplace add C:\path\to\lamor-plugins
```
