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
3. Запускать субагентов:
   - repo-fit;
   - bug-signal;
   - patchability.
4. Открывать `candidate-bug` issue до реализации.
5. Делать PR только когда есть repro или точный regression test.

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

