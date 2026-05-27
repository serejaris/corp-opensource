# Runner-контейнер для open-source scouting

## Зачем

Open-source scouting быстро превращается в набор тяжёлых checkout, зависимостей и долгих тестов. Для этого нужен отдельный runner на `corp-server`, чтобы:

- не загрязнять локальную машину;
- держать воспроизводимые repro-окружения;
- запускать несколько независимых проверок после subagent scouting;
- не использовать student/smoke CT как постоянный рабочий сервер.

## Целевое состояние

- Owner infra repo: `~/Documents/GitHub/corp-server`.
- Owner workflow repo: `~/Documents/GitHub/corp-opensource`.
- Host: `pc-sys2-pve01`.
- Target name: `corp-opensource-runner`.
- Target role: приватный LXC/VM для checkout, repro, targeted tests, GitHub CLI, Codex/Hermes checks.
- Public exposure: none by default.
- Secrets: не в git; только container env / host-managed secret path.

Точный CTID, IP, disk/RAM/CPU и lifecycle должны быть назначены через `corp-server` issue/runbook.

## Что нельзя

- Не использовать CT `216` как общий runner. CT `216` — smoke-test контейнер для Hermes/student-style проверок.
- Не запускать public services/DNAT без отдельного reviewed runbook и approval.
- Не хранить upstream tokens, OpenAI tokens, GitHub tokens или private keys в этом репозитории.
- Не считать локальный Mac repro достаточным, если баг зависит от Linux/container runtime.

## Provisioning gate

Перед созданием runner:

1. В `corp-server` есть issue/runbook с target CT/IP/resources.
2. Выполнен `corp-server-ops` workflow.
3. Есть dry-run/read-only проверка текущего Proxmox состояния.
4. Есть план rollback/cleanup.
5. Действие не трогает student CT и CT `216`.
6. Live mutation одобрена или явно проходит post-backup live-change policy `corp-server`.

## Runtime contract для задач

Для каждого candidate bug issue фиксируй:

- repo и upstream issue/PR;
- branch/checkpoint в runner;
- exact repro command;
- exact test command;
- результат: pass/fail и дата;
- что было очищено после проверки.

## Временное правило

Пока dedicated runner не создан:

- Hermes-specific checks можно запускать в CT `216`, если это bounded smoke/repro и после проверки не остаётся long-running process.
- General scouting/repro по `opencode`, `markitdown`, `firecrawl`, `gemini-cli`, `langchain` лучше держать локально или ждать runner, если зависимости тяжёлые.
