# Corp Open Source

Приватный контур для системной работы с open-source: какие upstream-репозитории смотрим, какие баги проверяем, какие PR ведём, где есть конкурирующие PR и какие уроки надо переносить в skills.

## Активный watchlist

| Проект | Upstream | Наш контур | Статус | Следующее действие |
|---|---:|---:|---|---|
| NousResearch/hermes-agent | [#32884](https://github.com/NousResearch/hermes-agent/pull/32884) | [fork branch](https://github.com/serejaris/hermes-agent/tree/fix/codex-null-output-stream) | Закрыт как duplicate после merge через [#32963](https://github.com/NousResearch/hermes-agent/pull/32963) | Проверить релиз, `openai-python#3315`, и hardening follow-up по broad `TypeError` recovery |

## Рабочие issues

- [#1 Track Hermes Codex null-output upstream fix](https://github.com/serejaris/corp-opensource/issues/1)
- [#2 Launch open-source bug contribution program](https://github.com/serejaris/corp-opensource/issues/2)
- [#3 Scout Hermes follow-up bugs after Codex null-output merge](https://github.com/serejaris/corp-opensource/issues/3)
- [#4 Scout opencode for reproducible coding-agent bugs](https://github.com/serejaris/corp-opensource/issues/4)
- [#5 Scout MarkItDown for fixture-friendly conversion bugs](https://github.com/serejaris/corp-opensource/issues/5)
- [#6 Hermes Codex recovery may mask unrelated TypeError after #32963](https://github.com/serejaris/corp-opensource/issues/6)
- [#7 MarkItDown IpynbConverter crashes on non-ASCII PDF](https://github.com/serejaris/corp-opensource/issues/7)
- [#8 opencode Task subagents omit tools for Bedrock/LiteLLM](https://github.com/serejaris/corp-opensource/issues/8)
- [#9 Firecrawl cancel crawl floods webhook failures](https://github.com/serejaris/corp-opensource/issues/9)

## Правила

- Репозиторий ведём на русском: README, playbooks, watch notes, issue bodies.
- Разведку по новым target repo делаем через субагентов:
  - repo-fit: активность, contribution rules, test surface, maintainer response;
  - bug-signal: свежие баги, дубликаты, реакции, боль пользователей;
  - patchability: где минимальный patch surface и есть targeted tests.
- Не начинаем PR без воспроизведения или понятного failing/regression test.
- Если есть competing PR, ведём duplicate triage публично: что сравнили, что портировали, чего не хватает.
- Реакции и лайки считаем impact-сигналом, но не approval и не CI.

## Playbooks

- [Стратегия open-source contribution](strategy.md)
- [Разведка багов в репозиториях](playbooks/repo-bug-scouting.md)
- [Гонка duplicate PR](playbooks/duplicate-pr-race.md)

## Метки

- `repo-scouting` — поиск и оценка репозиториев.
- `candidate-bug` — потенциальный баг для PR.
- `needs-subagents` — нужна параллельная разведка.
- `needs-repro` — нет подтверждённого воспроизведения.
- `duplicate-triage` — есть конкурирующие PR.
- `release-followup` — проверить, дошёл ли fix до релиза.
