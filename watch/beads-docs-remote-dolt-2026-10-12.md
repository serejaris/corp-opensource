# beads: docs-раздел про удалённый Dolt и латентность

Дата: 2026-10-12

Upstream: https://github.com/gastownhall/beads (main `b630f96`, 2026-10-12; релиз v1.3.1, rc 1.3.2-rc.1/rc.2)

Trackers: бид `cos-e4x.2` (эпик `cos-e4x`); досье `corp-research/2026-10/2026-10-12-beads-latency-remote-dolt.md`; рабочая схема `corp-server/runbooks/beads-server.md` § «Mac: bdr и шим bd».

Статус: `PR-READY` по содержанию и дубликатам, наружу не идём до явного «да» founder'а и трёх pre-PR шагов ниже.

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
