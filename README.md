# Corp Open Source

Приватный контур для системной работы с open-source: какие upstream-репозитории смотрим, какие баги проверяем, какие PR ведём, где есть конкурирующие PR и какие уроки надо переносить в skills.

## Execution model

- Разведка и triage идут через 6 субагентов, если есть несколько repo/PR или тема быстро меняется.
- Repro, targeted tests и тяжёлые checkout должны уходить в dedicated runner на `corp-server`: `corp-opensource-runner`.
- Пока runner не создан, CT `216` / `pc-hermes-test` можно использовать только для Hermes-specific bounded smoke/repro checks. Это не общий runner.
- Provisioning runner-контейнера живёт в `corp-server`; этот repo хранит очередь, watch notes и решения по upstream.

## Активный watchlist

| Проект | Upstream | Наш контур | Статус | Следующее действие |
|---|---:|---:|---|---|
| NousResearch/hermes-agent | [#32884](https://github.com/NousResearch/hermes-agent/pull/32884) | [fork branch](https://github.com/serejaris/hermes-agent/tree/fix/codex-null-output-stream) | Закрыт как duplicate после merge через [#32963](https://github.com/NousResearch/hermes-agent/pull/32963) | Проверить релиз, `openai-python#3315`, и hardening follow-up по broad `TypeError` recovery |
| pydantic/pydantic-ai | [#5678](https://github.com/pydantic/pydantic-ai/pull/5678) | [#14](https://github.com/serejaris/corp-opensource/issues/14) | Open, mergeable, CI green | Watch maintainer/bot review |
| pydantic/pydantic-ai | [#5680](https://github.com/pydantic/pydantic-ai/pull/5680) | [#16](https://github.com/serejaris/corp-opensource/issues/16), [watch](watch/pydantic-ai-5679-textcontent-metadata.md) | Open, mergeable, CI green | Watch maintainer/bot review |
| trycua/cua | [#1725](https://github.com/trycua/cua/issues/1725) | [#15](https://github.com/serejaris/corp-opensource/issues/15), [watch](watch/trycua-1725-windows-click-marker.md) | Candidate, no open duplicate PR found; Windows/runtime proof needed | Wait for runner/repro path before PR |

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
- [#10 Provision corp-opensource runner container on corp-server](https://github.com/serejaris/corp-opensource/issues/10)
- [#11 Weekly AI-native framework top scan](https://github.com/serejaris/corp-opensource/issues/11)
- [#12 Design Hermes/Codex worker pool for parallel scouting](https://github.com/serejaris/corp-opensource/issues/12)
- [#13 Scout Paperclip-like harness expansion opportunities](https://github.com/serejaris/corp-opensource/issues/13)
- [#14 pydantic-ai serialize_any loses binary data](https://github.com/serejaris/corp-opensource/issues/14)
- [#15 trycua Windows element-index clicks miss click.png marker](https://github.com/serejaris/corp-opensource/issues/15)
- [#16 pydantic-ai TextContent metadata lost in UI adapter round-trip](https://github.com/serejaris/corp-opensource/issues/16)

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
- [Repo scope: AI-native frameworks and harnesses](watch/repo-scope-ai-native-frameworks.md)
- [Разведка багов в репозиториях](playbooks/repo-bug-scouting.md)
- [Гонка duplicate PR](playbooks/duplicate-pr-race.md)
- [Follow-up по старым upstream PR/issues](playbooks/upstream-followup-loop.md)
- [Runner-контейнер на corp-server](playbooks/runner-container.md)

## Метки

- `repo-scouting` — поиск и оценка репозиториев.
- `candidate-bug` — потенциальный баг для PR.
- `needs-subagents` — нужна параллельная разведка.
- `needs-repro` — нет подтверждённого воспроизведения.
- `duplicate-triage` — есть конкурирующие PR.
- `release-followup` — проверить, дошёл ли fix до релиза.
