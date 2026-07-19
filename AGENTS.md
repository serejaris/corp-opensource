# corp-opensource

## Назначение

`corp-opensource` — рабочий контур для двух внешних дорожек:

1. системный open-source contribution: scouting, triage, upstream watch, PR evidence, duplicate-race notes и lessons learned;
2. безопасный публичный релиз артефактов: provenance, licensing, security/privacy, allowlist packaging, outward approval и fresh public verification.

Creator/community distribution, product evaluation, sales и infrastructure остаются у профильных owners; `corp-opensource` владеет contribution/release gates.

## Язык

- Файлы репозитория, issue bodies и internal notes веди на русском.
- Upstream PR/issue comments пиши на языке upstream repo, обычно на английском.
- Не копируй длинные upstream обсуждения; оставляй ссылки и короткий вывод.
- Public artifacts могут быть на английском или двуязычными по аудитории; private paths/internal context в них не переносить.

## Обязательные skills

- Для поиска репозиториев, candidate bugs и triage используй `open-source-bug-scouting`.
- Для подготовки/мониторинга upstream PR используй `open-source-pr-workflow`.
- Для публикации, open-sourcing, export или sanitization private/vendor/runtime artifacts используй `safe-public-release`.
- Если есть 2+ repo, 2+ competing PR или пользователь просит subagents/scouting/triage, запускай цикл из 6 субагентов и фиксируй synthesis в issue/watch note.
- Перед upstream comment/PR, который меняет статус кандидата, после parent live gates запускай 3-subagent critique: фактология/дубликаты, process gates, actionability. Результат фиксируй в issue/watch note.
- Перед outward public release обязательны inventory/provenance/license/security allowlist и отдельный approval dry run. Не создавать public repo и не менять visibility на этапе extraction/review.

## Runner Container

- Тесты, repro, heavy scouting и изолированные release scans должны выполняться в dedicated runner-контейнере на `corp-server`, когда это требует стабильного окружения или долгих зависимостей.
- Целевой runner: `corp-opensource-runner` на `pc-sys2-pve01`; точный CTID/IP назначает `corp-server`.
- До provisioning dedicated runner допускается использовать CT `216` / `pc-hermes-test` только для Hermes-specific smoke/repro checks. Не превращай CT `216` в общий open-source или release runner.
- Любое создание/изменение Proxmox CT — live infra mutation. Работай через `corp-server-ops`, issue/runbook в `corp-server`, dry-run/read-only gate и approval policy.
- В runner не клади приватные токены в repo. Секреты только в окружении контейнера или host-managed secret path.
- Release staging tree создавай в clean temporary/worktree directory из explicit allowlist. Не копируй целиком private/runtime directory.
- Если server runner работает в goal-mode/hourly-mode, hourly cadence — это триггер/heartbeat, а не обязанность коммитить. Commit+push делай только в конце завершённого scouting/follow-up/release-review цикла: required gates, tracker/playbook update, затем `git diff --check` и один bundled commit. Если цикл не завершён или изменений нет, commit не создавать.
- Для долгой автономной работы запускай Codex в его собственном goal/цель-режиме как один самостоятельный долгоживущий процесс. Не подменяй goal-mode bash-loop'ом, который постоянно перезапускает `codex exec`: такой wrapper можно использовать только как аварийный watchdog/heartbeat и только с явной пометкой в runbook/status.
- Hourly runner не открывает внешний upstream PR и не выполняет outward public release автоматически. Upstream PR остаётся ручным/явно подтверждённым действием после `PR-READY`; public release — после `PACKAGE-READY`, dry run и explicit approval.
- Если upstream PR закрыт без merge, superseded, rejected, assignment-blocked или стал stale/no-go, после фиксации evidence в `corp-opensource` удали лишний fork/repository/branch из аккаунта `serejaris`, чтобы GitHub не засорялся. Перед удалением проверь, что: PR outcome записан в README/watch/tracker; полезный diff сохранён ссылкой/patch или признан ненужным; нет открытых PR из этого fork/repo; repo не является самостоятельным рабочим проектом. Если GitHub API не даёт удалить repo, создай cleanup task в tracker и пометь `cleanup-pending`.

## Рабочий порядок: upstream contribution

1. Создай или обнови issue в `corp-opensource`.
2. Проведи scouting/triage через нужные subagent роли.
3. Parent live gates: issue state, assignee/labels, linked PRs/timeline, PR search, contribution docs, regression card.
4. Проведи 3-subagent critique и выбери ровно один `next_status`: `LEAD`, `CANDIDATE`, `COMMENT-FIRST`, `PR-READY`, `PR-OPEN`, `WATCH`, `NO-GO`.
5. Запусти repro/test в runner-контейнере или явно напиши, почему runner ещё не доступен.
6. Только после repro и `PR-READY` переходи в upstream PR workflow.

## Рабочий порядок: public artifact release

1. Создай owner epic, inventory/review child и publish/verify child.
2. Собери `release-manifest.yaml`: source, version, license, modifications, bundle files, exclusions, decision.
3. Проведи provenance и redistribution gate; unknown означает blocked.
4. Проведи manual security/privacy classification и staged-tree scans.
5. Собери clean package только из explicit allowlist.
6. Покажи release dry run: target, allowlist, exclusions, licenses, checks, fresh-clone plan, risks.
7. Получи explicit owner approval для точного public target и artifact set.
8. Опубликуй из clean staging tree.
9. Выполни fresh public clone/install/discovery smoke и повторные scans.
10. Назначь maintenance owner, next review и обнови playbook/skill уроком.

Canonical playbook: `playbooks/safe-public-release.md`.

## Проверка перед commit

```bash
git diff --check
```
