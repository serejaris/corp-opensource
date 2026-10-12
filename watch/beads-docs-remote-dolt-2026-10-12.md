# beads: docs-раздел про удалённый Dolt и латентность

Дата: 2026-10-12

Upstream: https://github.com/gastownhall/beads (main `b630f96`, 2026-10-12; релиз v1.3.1, rc 1.3.2-rc.1/rc.2)

Trackers: бид `cos-e4x.2` (эпик `cos-e4x`); досье `corp-research/2026-10/2026-10-12-beads-latency-remote-dolt.md`; рабочая схема `corp-server/runbooks/beads-server.md` § «Mac: bdr и шим bd».

Статус: `PR-READY` (12.10, после блока `Merge history`). Наружу только после явного «да» founder'а на issue и PR; пересказ «да» через оркестратора не засчитан (брифы 12.10).

Коррекция 12.10 (два шага): (1) #7223 (05.10) сделал linked issue обязательным; (2) #7323 (06.10) вернул это в «рекомендуется» (подробно в `Merge history`). Итог: для нашего PR issue не обязателен, но желателен (70 строк новой архитектурной документации с замерами). Формат тела PR берём у влитых внешних docs-PR. См. `playbooks/upstream-followup-loop.md` § Lesson registry.

Upstream actions: `0`

Runner actions: `0` (docs-only, repro не нужен; замеры — оркестратор на Mac, 12.10)

Черновик раздела (англ.): [beads-docs-remote-dolt-2026-10-12-section-draft.md](beads-docs-remote-dolt-2026-10-12-section-draft.md)

## Коротко

В docs апстрима нет ни слова про задержку bd при удалённом Dolt-сервере и про паттерн «bd рядом с базой». Открытых PR и issues на такой раздел или на ssh/remote-exec обёртку нет. Проблему апстрим знает как perf-баги (#6114, #4102, #5965, #5273, #3239), но документацией никто не закрывает. PR в `docs/` принимаются без предварительного issue (CONTRIBUTING: docs tweaks идут сразу PR'ом).

## Parent live gates (проверено 2026-10-12)

- Свежий клон `gastownhall/beads` (depth 50), `docs/` просмотрен целиком. Grep по `latenc|round-trip|RTT|remote dolt|proxied|ssh`: латентность по сети нигде не описана. Единственное близкое — `docs/multi-agent/federation.md:242` (удалённый sql-server упомянут как топология для идентичности узла) и `docs/architecture/dolt.md` § Server Mode (таблица `BEADS_DOLT_SERVER_HOST`, про скорость ничего). `docs/reference/troubleshooting.md` § Performance Issues — только размер базы и compact.
- Разделов `core-concepts/sync-concepts`, `multi-agent/*`, `getting-started/*`: про удалённый сервер и латентность нет. Отдельной страницы про proxied-server в docs тоже нет (только упоминания в dolt.md, troubleshooting, upgrading).
- Поиск по issues/PR с 2026-08-01 (`gh search`, запросы: latency, remote dolt, ssh wrapper, remote exec, round trips, WAN, RTT, tunnel, BEADS_DOLT_SERVER_HOST, ControlMaster, bd serve, proxied-server docs, docs server mode, docs latency, docs remote server): ни одного PR/issue про docs-раздел или ssh/remote-exec обёртку.
- Смежное, не дубль:
  - perf-issues: #6114 (preamble 44–48 round-trips, bootstrap-cache), #5965 (3 соединения), #5273 (список сокращений), #3239, #4102, #6116, #6117 (write-транзакции при 80 мс RTT).
  - perf-PR: #6364, #6360, #6361, #5993 (skip preflight для remote TCP), #7297 (schema probe SHOW COLUMNS).
  - docs-issue #7397: релиз-ноты 1.3.1 vs реальный default `bd init` (embedded vs proxied). Касается default-топологии, нашего раздела не покрывает; в тексте default не утверждаем.
- Правила для docs-PR: `CONTRIBUTING.md` (docs tweaks сразу PR, `type(scope): summary`, `docs:` conventional; `make check-docs`, `make test`; в PR-шаблоне поля Why / What changed / Evidence it works / Checklist), `CONTRIBUTING_PR_GUIDELINES.md` (докажи проблему на stock beads без оркестратора; для perf-утверждений нужны числа; PR небольшой, один слой). Пользовательские docs живут в `docs/` (Mintlify, `docs/docs.json`).

## Куда класть

Подраздел в `docs/architecture/dolt.md` после «Server Mode», перед «Maintenance». Запасной вариант: `docs/architecture/remote-server.md` + строка в `docs/docs.json` (Architecture). Плюс указатель из `docs/reference/troubleshooting.md` § Performance Issues. Подраздел минимальнее: нет новых страниц, нет правок навигации.

## Safe-public-release проход по черновику

Текст черновика: без хостов, пользователей, путей, имён репо и IP. Хост `dolt-host.example.com` и путь `~/work/myproject` — плейсхолдеры. Из наших материалов взяты только числа замеров и номера публичных upstream-issues. grep по `citadel|serejaris|corp-|/Users|hq|IP|token|password` пуст. Приватные ссылки (досье, runbook, имя шима `bdr` как нашей реализации) остались только в этой watch-заметке, не в черновике; обёртка названа `bdr` как нейтральное имя примера.

## Чем подтверждены утверждения черновика

- Замеры 9,9 с / 0,85 с / 0,17 с, RTT 0,24 с: досье corp-research, bd 1.3.0. Не перемерены на 1.3.1.
- 44–48 round-trips и `bd close` 150: #6114 (bd 1.2.1). `bd ready --limit 1` ~48 → ~21: #6731 (слит после 1.3.1).
- Флаги `-C`, `--actor`, `--stdin`, `--body-file -`: `bd --help` и `bd create --help` на 1.3.0, плюс `docs/CLI_REFERENCE.md` апстрима.
- Квотинг обёртки `printf '%q'`: проверен локально на строке с `;` и пробелами (подстановка корректна). Сам ssh-запуск на тестовом хосте для примера из черновика отдельно не гонялся; боевая версия — шим из corp-server.
- Ограничения (tty, файловые аргументы, хуки, actor): досье § «Ограничения удалённого исполнения», выведено из `bd help`.
- Proxied-server: DoltHub blog 2026-07-22; выигрыш по RTT нигде не измерен (поэтому в таблице альтернатив он без обещаний).

## Решение

`next_status: PR-READY` (docs-only). Дубликатов нет; риск гонки низкий: тему «как жить с удалённым Dolt» никто не заявил.

## Что остаётся до PR

1. Явное «да» founder'а на fork и PR (сейчас ничего наружу).
2. Перемерить на 1.3.1 (и по возможности на сборке main) те же три команды; если числа изменились, поправить таблицу и фразу про 1.3.1. Пример ssh-обёртки прогнать на тестовом хосте.
3. 3-subagent critique по AGENTS.md (фактология/дубликаты, process gates, actionability) перед комментарием/PR; результат записать сюда.
4. После «да»: fork `gastownhall/beads`, ветка `docs/remote-server-latency`, один коммит `docs(dolt): remote server latency and running bd next to the database`, `make check-docs`, PR по шаблону (Why = ссылки на #6114/#4102/#5273 + замеры; Evidence = таблица замеров и команды); `open-source-pr-workflow` на этапах fork/PR. После закрытия PR — cleanup fork по AGENTS.md.
5. Опционально и независимо от PR: короткий комментарий в #6114 со ссылкой на новый раздел, когда PR откроется (не раньше).

## Critique: process (2026-10-12, ось PROCESS GATES, только чтение)

Проверено по апстриму через `gh api` (CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md, Makefile, docs/docs.json, docs/architecture/dolt.md, открытые PR).

- docs-PR без issue: PASS. CONTRIBUTING: docs tweaks идут сразу PR'ом с «why» в описании; `Closes #` в шаблоне необязателен. DCO, CLA и sign-off в CONTRIBUTING нет; отдельных правил про AI-assisted вклад нет (раздел про AI описывает работу мейнтейнерских агентов), раскрытие авторства не требуется.
- Коммит и PR: PASS. Conventional `type(scope): summary`, `docs(dolt): ...` подходит. Шаблон: Closes # / What changed / Evidence it works / Checklist (`make test`, docs обновлены). Evidence = таблица замеров и команды; для docs-only в чеклисте явно написать, что `make test` не затронут.
- `make check-docs`: FIX. Это Bazel (`docsync_test` + `doc_freshness_test` + `scripts/check-doc-flags.sh`); без bazel брать `make check-docs-go`. Ни тот, ни другой не гонялся: прогнать на ветке до PR и вставить результат в Evidence.
- Место в дереве: PASS. `docs/architecture/dolt.md` уже в навигации (`docs.json`, группа Architecture), подраздел `###` встаёт между «Server Mode» (стр. 168) и «Maintenance» (стр. 230), правок `docs.json` не нужно. Ссылки черновика (`/core-concepts/sync-concepts`, `/multi-agent/bucket-federation`, `/getting-started/upgrading`) существуют. Запасной вариант с новой страницей потребует строки в docs.json, но минимальный путь её не требует.
- Дубли: PASS. Открытые PR 7447, 7511, 7514 про remote в смысле Git/Dolt-синка и CI, не про латентность; docs-PR 7448 правит другой абзац (топология по умолчанию), конфликта по строкам с dolt.md нет.
- Fork и ветка: BLOCK до «да» founder'а. `serejaris/beads` не существует (`gh repo view`: не найден). Шаги: «да» → `gh repo fork gastownhall/beads --clone=false` → ветка `docs/remote-server-latency` от свежего main → один коммит.
- Комментарий в #6114 (open, kind/chore, p2): FIX. Процессно допустим (ответ в issue, ничего не заявляем), но (1) числа `main` получены эмуляцией задержки прокси, перемер по сети и на 1.3.1 не сделан (шаг 3 заметки); (2) AGENTS шаг 5: репро в runner'е или явная запись, почему нет — записать «замер на Mac, runner не нужен»; (3) CONTRIBUTING_PR_GUIDELINES требует чисел для perf-утверждений: в тексте их достаточно, слово «upper bound» оставить.
- safe-public-release (текст и код): PASS. `dolt-host.example.com`, `~/work/myproject` плейсхолдеры; хостов, IP, путей, пользователей нет; в обёртке нет секретов. FIX мелкий: пример ssh-обёртки заявлен как рабочий, но на тестовом хосте не гонялся; до PR прогнать (проверен только `printf %q`).
- Гейты AGENTS.md: пройдены parent live gates, дубли, safe-public проход. Остались: другие две оси 3-subagent critique (фактология/дубликаты, actionability), перемер, ssh-тест примера, `make check-docs`, явное «да» на комментарий и отдельно на fork/PR. Upstream actions: 0.

Шаги до отправки комментария: 1) перемер main и 1.3.1, поправить таблицу; 2) закрыть две остальные оси critique; 3) «да» founder'а; 4) отправить, записать ссылку в watch-заметку. Шаги до docs-PR: 1) «да» на fork/PR; 2) прогон ssh-примера; 3) fork и ветка; 4) `make check-docs` (или `-go`); 5) PR по шаблону; 6) ссылку на PR добавить в #6114 только после открытия.

## Critique: facts (2026-10-12, ось фактология и дубликаты)

Вердикт: FIX (одна правка числа). Живой апстрим перепроверен gh api в тот же день, main `b630f96`.

Подтверждено: в docs по-прежнему нет раздела про задержку/удалённый Dolt (поиск `latency` по docs/ даёт только MCP-сравнение и метрики); с 05.10 docs-коммиты (upgrading, ide-setup, cursor) тему не трогают; открытых PR/issues на такой раздел нет (ближайшие docs-PR #7448 и issue #7397 о default-топологии, не пересекаются); якоря в dolt.md есть («Server Mode (Multi-Writer / Orchestrator)», далее «Maintenance»), страницы sync-concepts, bucket-federation, upgrading и troubleshooting § Performance Issues существуют; флаги `-C`, `--actor`, `--stdin`, `--body-file -`, `create-form`, `edit`, `show --watch`, `--proxied-server` есть в `bd` 1.3.0; `dolt.host` в config.yaml и `BEADS_DOLT_SERVER_HOST` в dolt.md есть; RTT в issues (0.22 / 100-200 мс / 0.16-0.2 с) и 44-48 / `bd close` 150 верны; приватных данных нет.

FIX:
1. «`bd ready --limit 1` from about 48 to about 21 statements» из #6731: 48 → 21 это худший случай (issues + wisps + deferred epic + `--include-ephemeral`); по остальным конфигурациям 28 → 18, 34 → 19, 46 → 20. Написать «from 28-48 to 18-21 statements depending on the store».
2. Таблица 9.9 / 0.85 / 0.17 с: это наш замер на 1.3.0 (одна точка). Добавить «a single run on bd 1.3.0» перед таблицей, чтобы не выглядело как усреднение.
Остаётся на авторе: перемер на 1.3.1 и прогон ssh-примера на тестовом хосте (в заметке уже в списке «до PR»).


## Статус PR-READY (2026-10-12)

Внесено: число #6731 заменено (наше 44 на main противоречило «18-21»: даём ссылку на #6731 и наши счётчики 24/40 с пометкой «in our run»); таблица помечена версиями, 3 прогонами и main b630f96; строка proxied-server проверена по `bd init --help` 1.3.0 и main (флаги `--proxied-server-external-host/-port` есть, EXPERIMENTAL) и исправлена; ssh-обёртка прогнана буквально (см. critique actionability).

Локальная ветка `docs/remote-server-latency` в `scratchpad/oss/beads-docs` (от main b630f96), коммит ca52db3, в апстрим не пушилась. Patch: [beads-docs-remote-dolt-2026-10-12.patch](beads-docs-remote-dolt-2026-10-12.patch). `make check-docs-go`: exit 0 (check-doc-flags, check-doc-freshness, `go test ./test/docsync` ok). Bazel нет, `make check-docs` не гонялся.

### (устарело 12.10, см. «PR: title/body» ниже) Финальный PR: title

```
docs(dolt): remote server latency and running bd next to the database
```

### (устарело 12.10) Финальный PR: body

````markdown
Refs #6114, #4102, #5273 (no `Closes`: this documents a workaround, it does not fix the latency).

## What changed

Adds a subsection to `docs/architecture/dolt.md`, between "Server Mode" and "Maintenance": why every bd command costs one round-trip per SQL statement against a remote Dolt server, measured numbers, a minimal ssh wrapper that runs `bd` on the Dolt host, its limits, and a table of alternatives. Docs only, no code, no `docs.json` change. Why: the docs say nothing about latency against a remote server, while the problem is reported repeatedly (#6114, #4102, #5273, #3239).

## Evidence it works

Measured against a Dolt server on another continent (RTT 0.24 s through an SSH port forward), `bd show <id>`, median of 3 runs per row:

| Where bd runs | 1.3.0 | 1.3.1 | `main` (b630f96) |
|---|---|---|---|
| Laptop, Dolt server remote | 10.2 s | 10.3 s | 10.8 s |
| Dolt host, via reused SSH connection (the wrapper in the text, run as written) | 0.73 s | - | - |
| Dolt host, local shell | 0.17 s | - | - |

The wrapper was run as written: `ready --json`, `list`, `show`, `create --stdin` (body returned byte for byte, quotes and `$HOME` intact), `--body-file -` work; `--body-file relative.md` fails on the remote side, as the text says. The first call without an SSH master took 5.3 s. Statement counts quoted in the text (24 for `bd list --limit 1`, 40 for `bd show` on `main`) come from a delaying TCP proxy in front of a local `dolt sql-server --loglevel debug`, counting "Starting query" lines.

Docs checks: `make check-docs-go` passes (flag check, freshness check, `go test ./test/docsync`). Bazel is not installed here, so `make check-docs` was not run.

## Checklist

- [x] The why is documented: in this description, with links to #6114, #4102, #5273.
- [x] `make test` is not affected (docs only); `make check-docs-go` passes.
- [x] User-facing behavior changes are reflected in `docs/`: this PR is the docs change.
- [x] No invariant or rule changed, so no `AGENTS.md` update.
````

## Merge history (12.10.2026, по playbooks/repo-bug-scouting.md § «История влития»)

Источник: живой GitHub `gastownhall/beads`, проверено 12.10.2026 через `gh pr list/view`, `gh api`.

### 1. Последние 50 влитых PR

- Окно: 09.10 07:42 UTC - 12.10 09:06 UTC (трое суток).
- Авторы: `julianknutsen` 44 (мейнтейнер), `app/gascityinc-randy` 3 (бот мейнтейнеров), `bee-ghosttrack` 2, `steveyegge` 1. Внешних людей в окне 2 из 50 (4 %): оба PR от `bee-ghosttrack` - #7423 (`fix(bazel)`) и #7454 (`test(show)`); третий внешний - `steveyegge` (#7460, владелец проекта). Окно короткое и перекошено потоком мейнтейнера, поэтому ниже взята выборка docs-PR за июль - октябрь.
- Время от открытия до merge, 50 PR: медиана 2,3 ч, от 0,1 до 26 ч.
- Внешние docs-PR, влитые за 16.07-10.10 (`gh pr list --state merged --search "docs in:title"`): #7333 и #7132, #7268, #7382 (bee-ghosttrack), #7280, #7284, #7301, #7310 (csells), #6726 (vishnujayvel), #6413 (ksmeltzer), #6001, #6431 (quad341), #6688, #5714, #5852 (postoso), #6432, #6318, #5501 (ecuthiell), #5838, #6239 (cuongbphv), #6446, #6502, #6332. То есть внешние docs-PR вливаются регулярно, чаще всего без закрывающего issue.

### 2. Образцы внешних docs-PR (прочитаны целиком)

| PR | Автор | Заголовок | Размер | Тело |
|---|---|---|---|---|
| #7280 | csells | `docs(upgrading): adopt a migrated remote without relying on bd bootstrap` | +81/-29 | `Part of #6687 ...` в первой строке; `## Problem`, `## Change (docs only)`, `## Validation` («docs-only; Go-тесты не гонялись»); без closing issue |
| #7284 | csells | `docs(upgrading): reads fail too ...` | +7/-5 | `Refs #6929, #7038. Follows #7280.`; Problem, Change (docs only), Validation |
| #7301 | csells | `docs: scope the shared-server read failure ...` | +29/-8 | `Refs #6929`; нумерованные секции; таблица; Validation; одобрил Bee, авто-триаж `kind/docs p3` |
| #6726 | vishnujayvel | `docs(config): document claim.pools ... (#6607)` | +18 | `## What`, `## Why` (`Fixes #6607`), `## Verification` с командами (`check-doc-freshness.sh`, `go test ./test/docsync`, `check-doc-flags.sh`); `<details>` Evidence & notes |
| #6413 | ksmeltzer | `docs(schema): document the provenance invariant ...` | +5 | What / Why / Verification; в конце подпись `_opencode-glm-5.3-flash-unknown-reasoning on behalf of ksmeltzer_`; мейнтейнер принял «as-is» (Adoption Review) |
| #6001 | quad341 | `docs(testutil): document migration-0032 hang ...` | +59 | Summary / Test plan; ревью просили перенести заметку из комментария в `engdocs/TESTING.md`, т.е. длинная проза живёт в docs, не в коде |
| #7132 | bee-ghosttrack | `docs(show): say that --json omits comments ...` | +3/-1 | What / Why / Not in this PR / Tested; в конце `🤖 Generated with Claude Code` |

Общее: conventional-заголовок `docs(scope): ...`; первая строка `Refs #N`/`Part of #N`/`Fixes #N`, когда связанный issue есть; ключевые секции Problem/Why и Change/What; блок проверок с точными командами и честной пометкой «Go-тесты не гонялись» для docs-only; таблицы приветствуются; раздел «Not in this PR» у тех, кто просит узкого ревью.

### 3. Изменения правил за 60 дней (13.08-12.10)

`gh api repos/gastownhall/beads/commits?path=...&since=2026-08-13`:

| Дата | PR | Что |
|---|---|---|
| 01.10 | #5614 | CONTRIBUTING: release-version checks без shell |
| 03.10 | #7147 | CONTRIBUTING: advisory workflows (Blacksmith, фильтры путей) |
| 05.10 | #7223 | `docs(agents)`: contributor-only agent instructions и «documented-issue intake»: linked issue обязателен для каждого PR; формы issue с полями motivation/impact/risk/verification/size/intent/AI disclosure/accountability, `NOT_ENOUGH_INFO` для агентов |
| 06.10 | **#7323** | `docs(contributing)`: «recommend issues when they add context instead of requiring one per PR». Issue рекомендуется для user-visible багов, изменений поведения и работы на несколько PR; «typos, flaky tests, refactors, CI or docs tweaks» идут сразу PR'ом с «why» в описании. В CI и rulesets обязательность не проверялась (`scripts/pr-preflight.sh` лишь предупреждает) |
| 07.10 | #7341, #7348, #7357 | Bazel становится основным путём тестов (nogo вместо go vet/golangci-lint; `make test` = `bazel test`) |

То есть утверждение «#7223 требует issue» верно только для окна 05-06.10; с 06.10 действует #7323. Это поправка к первой записи 12.10.

### 4. Требования шаблона PR, форм issue, раскрытие AI

- PR-шаблон (main): `Closes #` с комментарием «рекомендуется для user-visible багов и изменений, опционально для малых», `## What changed`, `## Evidence it works` («`unit tests pass` alone is not evidence»), чеклист из 4 пунктов (why documented; `make test` и lane из `engdocs/TESTING.md`; docs обновлены; `AGENTS.md` при смене инварианта). `engdocs/TESTING.md` для docs-only: `git diff --check` и `make check-docs`, полный suite не нужен.
- Форма feature (`feat: `): обязательные Motivation, Proposed change, Impact, Risk and compatibility, Verification plan, Estimated size, Intent, AI assistance (`No` / `Yes, partly` / `Yes, mostly`), Accountability («A human has read this issue and stands behind it»); необязательно Alternatives considered. Для docs-issue формы отдельной нет: docs-issue подают через bug (#7387, #7385, #7384) или feature (#7313) с префиксом `docs:`.
- AI-раскрытие у принятых PR: `engdocs/AGENT_SIGNING.md` просит подпись `_{runtime}-{model}-{reasoning} on behalf of {user}_` в комментариях и трейлер `Agent-Signature:` в коммитах агентов; модель/эффорт не угадывать (`unknown-model`). Внешние авторы: #6413 подпись `_opencode-glm-5.3-flash-unknown-reasoning on behalf of ksmeltzer_`; #7132 и #7382 `🤖 Generated with Claude Code`; csells/vishnujayvel явной пометки не ставят. Раскрытие не блокирует merge.

### 5. Какие проверки CI засчитывает

- Ruleset `beads main: required CI gates` (merge queue): обязательны `CI Gate / Required` и `PR Risk Gate / Required`. Для образца (#7497) в этих гейтах зелёные `Bazel / test`, `Bazel / integration`, `Check doc flags freshness`, `Check doc freshness`, `PR preflight process`, `Fast checks`; доп. для docs-PR: `Docs nav/link sync (docsync guard)` и `Mintlify broken-links (baseline-aware)`.
- `make check-docs` (Bazel: `//test/docsync:docsync_test`, `//scripts/repochecks:doc_freshness_test`, затем `scripts/check-doc-flags.sh`) соответствует CI; `make check-docs-go` - локальный аналог с `go test`, CI его не гоняет. Прогон Bazel см. ниже.
- Ложно-красные: у PR от форка часть джобов падает за 7-8 с на запуске (#7301: десятки fail по 8 с), мейнтейнеры одобряют по содержанию; ждать нужно `CI Gate / Required` после approve workflows.

### Вывод

Формат нашего PR: заголовок `docs(dolt): ...`; `Closes #<NEW>` (issue заводим, потому что раздел с замерами и спорными рекомендациями «worth discussing»; обязательным он не является); секции Problem, Change (docs only), Evidence it works, чеклист шаблона; одна строка AI-подписи по `AGENT_SIGNING.md`. Если founder предпочтёт не заводить issue: убрать `Closes`, оставить `Refs #6114, #4102, #5273`, формат остальных блоков тот же.

## Bazel `make check-docs` (12.10.2026)

Поставлен `bazelisk` 1.29.0 (`brew install bazelisk`, Bazel 9.3.0 по `.bazelversion`). В клоне на ветке `docs/remote-server-latency` (коммит ca52db3, поверх main b630f96): `make check-docs` **exit 0**, 4 мин 40 с на холодном кэше (macOS arm64, локальное исполнение, без remote-exec). `Executed 2 out of 2 tests: 2 tests pass` (`//test/docsync:docsync_test`, `//scripts/repochecks:doc_freshness_test`), затем `scripts/check-doc-flags.sh` против pin v1.2.2: `PASSED`. WARN про legacy `.beads/*.db` в `dolt.md:984` и `upgrading.md` - старые строки, не наши. `git diff --check` чист. Правок раздела под стиль не потребовалось (ссылки в стиле `](/core-concepts/...)`, заголовки `###`/`####`, как в соседних разделах), нового коммита нет, patch не менялся (сверен с `git format-patch -1`).

## Issue: title

```
docs: document latency against a remote Dolt server and running bd next to the database
```

Форма: feature (`feat: ` в форме заменён на `docs: ` как у #7313/#7387; метки не ставим, триаж сделает автоматика).

## Issue: body

`````markdown
### Before you continue

- [x] I searched existing issues and pull requests and did not find a duplicate.

### Motivation

The docs say nothing about what bd costs against a Dolt server on another machine. Every bd command is a short-lived process that runs its SQL statements one after another, so the network round-trip time (RTT) is paid once per statement. Reports of this: #6114 (44 to 48 awaited round-trips even for the cheapest commands on 1.2.1), #4102, #5273, #3239.

Measured with bd 1.3.0 against a Dolt server on another continent (RTT 0.24 s through an SSH port forward), `bd show <id>`, median of 3 runs, same issue every time:

| Where bd runs | `bd show <id>` |
|---|---|
| Laptop, Dolt server remote | 10.2 s |
| On the Dolt host, started over a reused SSH connection | 0.73 s |
| On the Dolt host, in a local shell | 0.17 s |

bd 1.3.1 gave the same numbers within 0.1 s; a build of `main` (b630f96) took 10.8 s. Details and statement counts are in [my comment on #6114](https://github.com/gastownhall/beads/issues/6114#issuecomment-6128075017). People who hit this today have no documented way to tell that RTT is the cause, and no documented workaround.

### Proposed change

A docs-only subsection in `docs/architecture/dolt.md`, after "Server Mode", that covers:

- why the cost scales with RTT and not with the size of the result, and how to recognise it (slow only against a remote server, fast with the server on the same machine);
- the measured numbers above, with versions and RTT;
- the pattern "run bd next to the database": a minimal SSH wrapper with connection sharing, and the details that matter (`bd` on the PATH of a non-interactive shell, `-C`, explicit `--actor`, stdin forwarding, file arguments read on the remote side);
- limits (commands that need a terminal, machine-bound commands, hooks, a second clone on the Dolt host, SSH access is shell access);
- a table of alternatives (proxied-server mode, embedded mode plus Dolt remote sync, a same-region server, waiting for the open perf work in #6114, #5965, #5273).

The patch is ready: https://github.com/serejaris/beads/commit/ca52db3ed2ba32e7f8185f7fb9789fd7c84716f2 (70 lines, one file).

### Impact

Users and agents that run bd from a laptop or CI against a shared Dolt server in another region. No change for anyone else. No code, `--json` output, or integration touched.

### Risk and compatibility

None for behavior: docs only, no schema, wire, or `--json` changes. The numbers are tied to bd 1.3.0/1.3.1 and one RTT, and the text says so; as the perf work in #6114 lands they will age, and the section tells readers how to re-measure. Rollback is a revert of the one commit.

### Verification plan

`make check-docs` (docsync and doc freshness tests, then the CLI flag check). The wrapper in the text was run as written against a real remote server: `ready --json`, `list`, `show`, and `create --stdin` work, and `--body-file` with a relative path is read on the remote machine, as the text says. The timings can be reproduced with `time bd show <id>` in each of the three setups.

### Alternatives considered

- Wait for the open perf work (#6114, #5965, #5273). Those reduce the statement count but, at this RTT, the delay stays in seconds (on `main`, `bd show` is still 40 statements in my run).
- Proxied-server mode: its documented goal is not fewer statements per command, and I did not measure it against a remote server; the section says so.
- Embedded mode plus Dolt remote sync: fast and offline, but one writer at a time.
- Put the text in `docs/reference/troubleshooting.md` instead: it is a pattern with a wrapper and limits, which fits the architecture page better; a pointer from troubleshooting can follow.

### Estimated size

S (under 200 lines)

### Intent

I am opening or will open a PR

### AI assistance

Yes, mostly

### Accountability

- [x] A human has read this issue and stands behind it.

AI disclosure: the text was drafted by Claude Code agents under my direction; the timings were measured on my own setup.

_claude-code-unknown-model-unknown-reasoning on behalf of serejaris_
`````

## PR: title

```
docs(dolt): remote server latency and running bd next to the database
```

## PR: body

`````markdown
Closes #<NEW>

Refs #6114, #4102, #5273, #3239. This documents a workaround; it does not fix the latency.

## Problem

`docs/architecture/dolt.md` explains server mode but says nothing about its cost against a Dolt server on another machine. Every bd command runs its SQL statements one after another, so the network round-trip time (RTT) is paid once per statement. #6114 counted 44 to 48 awaited round-trips for the cheapest commands on 1.2.1; #4102, #5273 and #3239 report the same at 0.1 to 0.25 s RTT. A reader who hits this has nothing in the docs that names the cause or offers a way out.

## Change (docs only)

One new subsection in `docs/architecture/dolt.md`, between "Server Mode" and "Maintenance" (+70 lines, no code, no `docs.json` change):

- What it costs: measured numbers with versions and RTT, and why the cost follows RTT and not the size of the result.
- Pattern "run bd next to the database": a minimal SSH wrapper with connection sharing, and the details that matter (`bd` on the PATH of a non-interactive shell, `-C`, explicit `--actor`, stdin forwarding, file arguments read on the remote side).
- Limits: commands that need a terminal, machine-bound commands, hooks, a second clone on the Dolt host, SSH access is shell access.
- Alternatives table: proxied-server mode, embedded mode plus Dolt remote sync, a same-region server, waiting for the open work in #6114, #5965, #5273.

## Evidence it works

Server in another region (RTT 0.24 s through an SSH port forward), `bd show <id>`, median of 3 runs per cell, same issue every time:

| Where bd runs | 1.3.0 | 1.3.1 | `main` (b630f96) |
|---|---|---|---|
| Laptop, Dolt server remote | 10.2 s | 10.3 s | 10.8 s |
| Dolt host, via reused SSH connection (the wrapper from the text) | 0.73 s | - | - |
| Dolt host, local shell | 0.17 s | - | - |

The wrapper was run as written: `ready --json`, `list`, `show`, and `create --stdin` (body returned byte for byte, quotes and `$HOME` intact) work, and `--body-file` with a relative path is read on the remote side, as the text says. The first call without an SSH master took 5.3 s. The statement counts quoted in the text (24 for `bd list --limit 1`, 40 for `bd show` on `main`) come from a delaying TCP proxy in front of a local `dolt sql-server --loglevel debug`, counting "Starting query" lines. More detail in [my comment on #6114](https://github.com/gastownhall/beads/issues/6114#issuecomment-6128075017).

Checks run on this branch (docs-only validation from `engdocs/TESTING.md`):

- `git diff --check`: clean.
- `make check-docs` (Bazel, 4 min 40 s on a cold cache, macOS arm64): exit 0. `//test/docsync:docsync_test` and `//scripts/repochecks:doc_freshness_test` pass, then `scripts/check-doc-flags.sh` against the pinned v1.2.2 prints `PASSED`.
- `make check-docs-go`: exit 0.

## Checklist

- [x] The why is documented: in the linked issue and in this description.
- [x] `make check-docs` (the docs lane in `engdocs/TESTING.md`) passes locally; `make test` is not affected, no code changed.
- [x] User-facing behavior changes are reflected in `docs/`: this PR is the docs change.
- [x] No invariant or rule changed, so no `AGENTS.md` update.

AI disclosure: the text was drafted by Claude Code agents under my direction; the timings were measured on my own setup.

_claude-code-unknown-model-unknown-reasoning on behalf of serejaris_
`````

Команды (только после явного «да» founder'а, полученного от него напрямую):

1. `gh issue create -R gastownhall/beads --title "<Issue title>" --body-file issue.md`
2. В теле PR заменить `<NEW>` на номер issue.
3. `gh pr create -R gastownhall/beads --head serejaris:docs/remote-server-latency --base main --title "<PR title>" --body-file pr.md`
4. Комментарий в #6114 про PR не нужен: шаблон и практика не требуют, issue и PR связаны через `Closes`/`Refs`.

Примечание: `engdocs/AGENT_SIGNING.md` просит трейлер `Agent-Signature:` у коммитов агентов; на ca52db3 его нет. Правка = переписывание уже запушенного коммита и force-push в форк; для внешнего вкладчика правило не обязательное (раздел про мейнтейнерские действия), поэтому не делали.

## Опубликовано (2026-10-12)

Статус: `PR-OPEN`. Решение founder'а в чате сессии: «не надо меня спрашивать сразу делай».

- Issue: https://github.com/gastownhall/beads/issues/7517
- PR: https://github.com/gastownhall/beads/pull/7518 (`Closes #7517`, head `serejaris:docs/remote-server-latency`, коммит ca52db3)
- Комментарий с замерами: https://github.com/gastownhall/beads/issues/6114#issuecomment-6128075017
- Подпись по engdocs/AGENT_SIGNING.md: `_claude-code-claude-opus-5-5-unknown-reasoning on behalf of serejaris_`.
- Следующий шаг: follow-up по playbooks/upstream-followup-loop.md — CI `CI Gate / Required`, `PR Risk Gate / Required`, ответы ревьюеров.

## 2026-10-12: HTTP backend добавлен в PR

Коммит 2a7149d в ветке docs/remote-server-latency: строка про HTTP backend (`bd serve` + `bd connect <url>`, #7288, #7469, #7471, #7497) в таблице альтернатив docs/architecture/dolt.md со ссылкой на раздел README «HTTP Backend (experimental)». Не в релизе (1.3.1 отказывает такой metadata.json, 1.3.2-rc.2 — хотфиксы), задержку по WAN не мерили, «быстрее» не заявлено. make check-docs: PASSED. Тело PR обновлено. CI: authorize и remove-label pass, Bazel Farm skipping (ждёт метки мейнтейнера).
