# Стратегия open-source contribution

## Цель

Системно находить важные open-source баги, воспроизводить их, чинить маленькими PR с тестами и превращать каждый исход в процессный урок.

## Какие репозитории подходят

Ищем репозитории, где:

- продуктом реально пользуются разработчики, AI builders или infra-команды;
- есть активность за последние 7 дней;
- баги можно воспроизвести локально или через fixture;
- есть понятный test harness;
- maintainers отвечают на issues/PR;
- маленький bugfix PR может быть принят без недельного design process.

Понижаем приоритет:

- спискам, awesome-репам, книгам и roadmaps;
- репам без свежих merged PR;
- огромным корпоративным репам без узкого воспроизводимого бага;
- задачам без тестовой поверхности.

## Основные дорожки

| Дорожка | Почему подходит | Примеры |
|---|---|---|
| AI agents / coding agents | Близко к нашей ежедневной боли, легко ловить outage и provider bugs | `NousResearch/hermes-agent`, `anomalyco/opencode`, `google-gemini/gemini-cli` |
| AI runtime frameworks | Много integration bugs и активных пользователей | `langchain-ai/langchain`, `langflow-ai/langflow`, `langgenius/dify`, `open-webui/open-webui` |
| Конвертация документов и scraping | Хорошо чинится через fixtures и regression tests | `microsoft/markitdown`, `firecrawl/firecrawl` |
| Self-hosted продукты | Хороши для Docker/runtime/release bugs | `n8n-io/n8n`, `supabase/supabase`, `immich-app/immich` |

## Цикл поиска бага

1. Сканировать свежие issues/PR:
   - `bug`, `regression`, `crash`, `TypeError`, `NoneType`, `broken`, `fails`;
   - много реакций/комментариев;
   - дубликаты;
   - регрессии после релиза.
2. Оценивать кандидата:
   - воспроизводится локально: `+3`;
   - затрагивает наши workflows: `+3`;
   - много реакций/дубликатов: `+2`;
   - маленький patch + test: `+2`;
   - maintainer active за 48 часов: `+2`;
   - огромный domain context: `-3`;
   - нет test harness: `-2`.
3. Запускать 6 субагентов:
   - repo-fit;
   - bug-signal;
   - repro-path;
   - patchability;
   - duplicate-race;
   - PR-readiness.
4. Открывать `candidate-bug` issue до реализации.
5. Делать repro в dedicated runner на `corp-server`, когда нужна Linux/container среда, тяжёлые зависимости или долгие тесты.
6. Делать PR только когда есть repro или точный regression test.

## Runner-контур

Целевой runtime для open-source работ: `corp-opensource-runner` на `corp-server`.

- `corp-opensource` отвечает за scouting, issue queue, watch notes, PR decisions.
- `corp-server` отвечает за CT/VM provisioning, ресурсы, lifecycle, backups, access policy.
- CT `216` / `pc-hermes-test` не является общим runner. Его можно использовать только как временный Hermes-specific smoke/repro контейнер.
- Создание или изменение runner-контейнера — live infra mutation и проходит через `corp-server-ops`, issue/runbook и approval policy.

## Ритм

- Раз в неделю: обновить candidate repos.
- Ежедневно: проверить активные upstream watch issues.
- При новом competing PR: duplicate triage через субагентов.
- После каждого исхода PR: обновить watch note и skill.

## Метрики

- найденные candidate bugs;
- воспроизведённые bugs;
- открытые PR;
- merged PR или landed upstream fixes;
- maintainer interactions;
- lessons, перенесённые в skills.
