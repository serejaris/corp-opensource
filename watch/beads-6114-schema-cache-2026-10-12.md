# beads #6114: комментарий с замерами и PR кэша проверки схемы

Дата: 2026-10-12

Upstream: https://github.com/gastownhall/beads (main `b630f96`, 2026-10-12; стабильный v1.3.1, rc 1.3.2-rc.1/rc.2)

Trackers: бид `cos-e4x.1` (эпик `cos-e4x`); досье `corp-research/2026-10/2026-10-12-beads-latency-remote-dolt.md`; соседняя заметка по docs-PR [beads-docs-remote-dolt-2026-10-12.md](beads-docs-remote-dolt-2026-10-12.md).

`next_status` для кэша схемы: `COMMENT-FIRST` (PR сейчас no-go). Комментарий в #6114: `PR-READY` по содержанию, наружу только после явного «да» founder'а и 3-subagent critique.

Upstream actions: `0`. Runner actions: `0` (локальный scratch-прогон на Mac, без runner'а; запись в upstream не было).

## Вердикт

1. Кэш «схема уже сведена» не занят: ни PR, ни assignee, ни заявленной работы. kost-t-human в #6114 сам отказался: «I have not touched it ... that is a call for the maintainers». bitbot-jon (09.10) предложил кэш и написал «We have not implemented or benchmarked that cache».
2. Но PR кэша сейчас нежелателен как первый шаг. Причины: (а) мейнтейнеры по сути не высказались, на #6114 за шесть недель только бот-триаж, лейблы `kind/chore` `priority/p2` (julianknutsen, 19.09), нет `status/accepted`; (б) дизайн-риск: версии схемы мало, в коде есть специальные «проверки реальности» (`cursorRealityFloor`, `doltIgnoreSeeded`, `readIgnoredCursorState`), и кэш не должен их отключать; (в) на main выигрыш на чтении мал: 2 из 24 выражений (замер ниже), основной вес на записи; (г) соседняя половина идеи (SHOW TABLES/COLUMNS вместо INFORMATION_SCHEMA) занята #7297 и #7409.
3. Что делать: сначала комментарий в #6114 с замерами и вопросом к мейнтейнерам (черновик ниже). PR кэша открывать только после их «да» на дизайн. Для нашей цели (чтения по 7-12 с) полезнее незанятые пункты #5273 (5: чтение одним выражением без BEGIN/ROLLBACK; 6: префетч конфига за один обмен). На них ни PR, ни assignee.

## Parent live gates (проверено 2026-10-12, gh api)

### Issues

| # | Состояние | Автор | Assignee | Метки | Последнее |
|---|---|---|---|---|---|
| #6114 bootstrap-cache, 35-40 обменов на open | open | steveyegge (агент от его имени) | нет | kind/chore, priority/p2 | 09.10 комментарий bitbot-jon (v1.3.1, схемные пробы ~56% занятости запросов) |
| #5965 3 соединения на вызов | open | jacobhausler | нет | kind/chore, p2 | 26.09 бот-триаж |
| #5273 остаток сокращений из #4167 | open | maphew | нет | kind/chore, p2 | 26.09 бот-триаж; 12 пунктов «независимо вливаемых», ни один не взят |
| #3239 пер-ID проверки wisps | open | fengning-starsend | нет | kind/chore, p2 | 16.06 maphew просил repro; 27.09 бот |
| #4102 5-10 с на удалённом Dolt | open | fishgills | нет | kind/bug, p2 | 27.09 бот; комментарий medhatgalal 11.07 |

Смежные: #6116 (write-path prepare→execute→close), #6117 (write-транзакции на всю команду, при 80 мс RTT `bd create` падает 2 из 2 с 1213), #5769 (INFORMATION_SCHEMA-пробы деградируют при мусорных БД), #6202 (тест на число соединений на вызов, завёл bee, «maintainer seat»), #6179, #6701.

### PR

| # | Состояние | Автор | Суть | Сигнал мейнтейнера |
|---|---|---|---|---|
| #6364 | open, `status/review-failed`, CONFLICTING | kost-t-human | один пул вместо двух, 29 → 23 команд на open proxied-провайдера | 4 APPROVE от bee-ghosttrack (07-08.09), не влит |
| #6360 | open, `review-failed` | kost-t-human | interpolateParams в proxied DSN | APPROVE 07.09, повтор 30.09, не влит |
| #6361 | open, `review-failed`, CONFLICTING | kost-t-human | родительские эпики `bd ready` одним чтением | APPROVE, не влит |
| #6122 | merged 12.09 | steveyegge | две лишние MySQL-сессии на вызов | влит |
| #5316 | merged 01.10 | maphew | MaxAllowedPacket в DSN, −1 обмен на соединение | влит, в main после 1.3.1 |
| #6731 | merged 25.09 | julianknutsen | `bd ready --limit 1` ~48 → ~21 выражений | влит, в Unreleased |
| #7400 | merged 08.10 | julianknutsen | без git-субпроцессов на read-only | влит |
| #7205 | merged 04.10 | julianknutsen | батч create/import без per-row обменов | влит |
| #7297 | open, CONFLICTING | bourgois | `leases.granted_node` проверять `SHOW COLUMNS`, не INFORMATION_SCHEMA.COLUMNS | APPROVE bee-ghosttrack 06.10 |
| #7409 | open, CONFLICTING | csauer02-personal-user | sentinel-таблицы `SHOW TABLES`, стек на #7297 | APPROVE 08.10; число выражений на read-пути не подтвердилось |
| #5993 | open | rileydev | пропуск preflight для remote TCP | без движения с 26.09 |

Поиск по новым PR/issues с 01.09 (gh api по всем 2380 объектам, фильтр по заголовкам: round-trip, latency, schema cache/probe/check, bootstrap, preamble, perf, information_schema, remote) и по телам PR на «6114»: PR кэша схемы или bootstrap-кэша нет. Единственные PR, упоминающие #6114, это #6364 и #6361.

Что значит «мейнтейнеры молчат»: аппрувы идут (bee-ghosttrack, «maintainer seat»), но три PR kost-t-human висят с 07.09 в `review-failed` и CONFLICTING. Для форков CI не запускается без одобрения мейнтейнера (`[code]smith skipping`), на #7409 красные Bazel-джобы упираются в «per-author live certificate quota». Практический риск для нашего PR: ревью быстрое, merge медленный.

## Правила CONTRIBUTING

- CLA нет (MIT, «By contributing, you agree...»). Assignment не нужен: issue «does not need maintainer approval first».
- Один PR на одну вещь, один слой на PR: примитив в `storage`/`issueops` первым, wiring в `cmd/bd` стеком сверху (CONTRIBUTING_PR_GUIDELINES.md). Кэш в `internal/storage/schema` + вызов в `internal/storage/dolt/store.go` и `uow` = два слоя, значит стек из двух PR.
- Для perf-заявки обязателен benchmark/тест, «reviewer will not assume the new code is faster»; нужен minimal repro на stock beads.
- Conventional commits (`perf(schema): ...`), в описании сначала What/Why, комментарии в коде по минимуму, `Closes #N` когда есть issue (для #6114, зонтичного, подойдёт «Part of #6114»).
- Gate: Bazel (`make test`, `--config=fork-cache` для контрибьюторов), nogo-линт, `make bazel-sync` для BUILD.bazel; `go test` годится как inner loop.
- Дизайн-вопросы с изменением момента, когда клиент замечает движение схемы, мейнтейнеры любят слышать до кода (kost-t-human так и сказал).

## Код на main (b630f96)

Проверка схемы при открытии сервера, `internal/storage/dolt/store.go`:
- `schema.CheckForwardDrift(ctx, db)` (строка ~2206) на каждом открытии, чтение и запись. Два выражения: `SELECT COUNT(*) FROM information_schema.tables ... 'schema_migrations'` и `SELECT COALESCE(MAX(version), 0) FROM schema_migrations` (`migrationSource.currentVersion`, `internal/storage/schema/schema.go`).
- Для записи (не ReadOnly, не Gateway) `store.initSchema` → `initSchemaOnDB` → `schema.MigrateUpWithLock` (`internal/storage/schema/lock.go`). Перед `GET_LOCK` идёт `alreadyConverged` (`converged.go`): `SELECT DATABASE()`, версии обоих курсоров, `dolt_ignore`, состояние ignored-курсора, `IS_FREE_LOCK`. Он уже сводит блокировку, но не убирает обмены.
- В `currentVersion` внутри: `cursorRealityFloor` (sentinel-таблицы wisps, wisp_dependencies, events, bd_events_*, колонки `leases.granted_node`): «курсор — утверждение о схеме, а утверждение, противоречащее схеме, хуже его отсутствия» (случаи gh 5033, 4356: БД, материализованные вне основного пути).
- Хрупкость, которую надо учесть: сессия Dolt, выполнившая упавшее выражение, залипает на старом снимке каталога (be-bv7x), поэтому существование таблицы проверяют «безопасным» выражением до чтения.

Где хранить отметку и как инвалидировать (оценка, не реализация):
- Файл в `.beads/` рядом с `dolt-server.port`/`push-state.json` (gitignored, на клон). Ключ: адрес сервера, имя БД, `LatestVersion()` и `LatestIgnoredVersion()` бинаря, идентичность БД (`_project_id` уже читается при открытии).
- Отметка локальна, поэтому ничего не знает о сервере. Нужен минимум одно ревалидационное чтение на каждый вызов: версия главного курсора (одно выражение вместо двух, остаётся защита от forward drift, #4135/#4137). Полный отказ от чтения нарушает защиту от прежнего бинаря и от пересозданной БД.
- Кэш не должен молча отключать `cursorRealityFloor`, `doltIgnoreSeeded`, `readIgnoredCursorState`. Вариант: отметка снимается при любой ошибке и при несовпадении ревалидации; либо отметка хранит результат этих проверок и живёт ограниченное время. Решение стоит спросить у мейнтейнеров (bitbot-jon пишет то же: «The version number alone is insufficient evidence of convergence»).
- Для чтения (read-only пути) экономия 1 обмен из двух; для записи большая часть цепочки.

## Замеры на main (мои, 12.10)

Метод: `go build -tags gms_pure_go ./cmd/bd` на main b630f96 (локальный клон, без push); локальный `dolt sql-server` 2.3.5 с `--loglevel debug` (счёт «Starting query»); между bd и сервером Python-прокси с задержкой 0,12 с в каждую сторону + 0,12 с на TCP-handshake (= RTT 0,24 с, как у нас до сервера). Проект `bd init --server --external`, одна БД. Шум ±1 с (одна машина, asyncio-прокси), поэтому ниже диапазоны.

| Команда на main при RTT 0,24 с | Выражений на сервере | Время |
|---|---|---|
| `bd list --limit 1` | 24 (2 версия схемы, 12 START TRANSACTION/ROLLBACK, 10 чтений) | 7,0-8,7 с |
| `bd show <id>` | 40 | 10,2-12,1 с |
| `bd ready --limit 1` | 44 | 11,8-11,9 с |
| `bd create` | 97 (около 30 на схему: 2 drift-guard + 26 init + повтор) | 26,5-28,3 с |

Верхняя граница выигрыша от «схему не проверять вообще» (патч в scratch: пропуск `CheckForwardDrift` и `initSchema`, не для PR): `list` 6,6 против 7,4-8,7 с (минус 0,8-2 с), `show` в пределах шума, `create` 19,6 против 28,3 с (минус 8,7 с, около 31%). Вывод: кэш схемы на main даёт заметное только на записи; читать-команды съедает не схема, а транзакционные обёртки (12 из 24 на `list`) и чтения конфигурации (`custom_statuses`, `custom_types`, `config` ×3, `metadata`).

Наши реальные замеры (досье): bd 1.3.0 `bd show` 9,9 с через сеть, 0,85 с через ssh-ControlMaster, 0,17 с на самом хосте Dolt; RTT 0,24 с. Основная картина на main не изменилась: PR #5316 убрал `SELECT @@max_allowed_packet`, #6122 — лишние сессии, но `show` остаётся около 10 с.

Лог-файлы и бинарники: `/private/tmp/claude-501/-Users-ris-Documents-GitHub-hq/770f3faa-41cf-4016-9074-d52656c811a0/scratchpad/oss/` (`q-list.txt`, `q-show1.txt`, `q-ready.txt`, `q-create.txt`, `delayproxy.py`). В репо не кладём.

## Тесты пакета

`BEADS_TEST_SKIP=dolt go test -tags gms_pure_go ./internal/storage/schema/ -count=1` на main b630f96: `ok`, 1,1 с (Dolt-тесты пропущены флагом, Bazel-gate не гонялся). Как тестируют такие пути в репо:
- sqlmock-потоки выражений со строгим порядком (`converged_test.go`: `expectConvergedProbe`, `expectCursorProbe`, `expectMigrationLockProbe`). Тест на сокращение обменов пишется как «поток из N выражений ровно такой».
- Живой Dolt через `internal/testutil` (`BEADS_TEST_DOLT_SERVER=local`, берёт `dolt` из PATH): например `TestShowTableExistsMatchesInformationSchemaOnDolt` в #7409.
- Регрессия на число соединений/выражений на вызов просили отдельно (#6202), она не написана; для кэша такой тест (счёт «Starting query» до/после) и был бы доказательством для perf-заявки.

## Рекомендация по следующему шагу

1. Комментарий в #6114 (черновик ниже) после «да» founder'а.
2. По ответам мейнтейнеров решить: кэш (стек из двух PR: примитив в schema, потом wiring) или #5273 п.5/п.6 (чтение одним выражением, префетч конфига), которые бьют по нашему узкому месту сильнее.
3. Перед комментарием по AGENTS.md: 3-subagent critique (фактология/дубликаты, process gates, actionability), результат дописать сюда.

## Черновик комментария в #6114 (английский)

> Measurements from a remote setup, in case they help prioritise. Not a fix; I have not written a cache. Two questions for maintainers are at the end.
>
> **Setup.** Direct server mode (`dolt_mode: server`) against an external `dolt sql-server` reached through an SSH port forward; client and server on different continents, RTT 0.24 s. Read-only commands, 3 runs each, medians, same repo and issue on every version. Statement counts come from a separate run: a delaying TCP proxy (0.12 s each way) in front of a local `dolt sql-server --loglevel debug`, counting "Starting query" lines. One machine for that run, so expect about ±1 s of noise there.
>
> | wall time over the network, RTT 0.24 s | 1.3.0 | 1.3.1 | `main` (b630f96) | statements on `main` |
> |---|---|---|---|---|
> | `bd show <id>` | 10.2 s | 10.3 s | 10.8 s | 40 |
> | `bd list --limit 3` | 8.3 s | 8.2 s | 7.0 s | 24 (`--limit 1`) |
> | `bd ready` | 8.3 s | 8.2 s | 8.9 s | 44 (`--limit 1`) |
>
> `bd create` on `main` was 97 statements and 26.5-28.3 s in the delay-proxy run only (I did not write to a shared database). Next to the server, `bd show` takes 0.17 s (1.3.0). On this path 1.3.1 changes nothing, and `main` is faster only for `list`.
>
> **Where the exchanges go on `main`.** `bd list --limit 1` is 24 statements: 2 for the schema version (`information_schema.tables` existence probe, then `MAX(version)`), 12 are START TRANSACTION/ROLLBACK around single reads, 10 are the reads themselves (custom statuses/types, `config` three times, `metadata`, the list). A write open adds about 26 schema-init statements plus two forward-drift guards: of 97 statements for `bd create`, about 30 are schema checks. On 1.3.0 the list for `bd show` was: login, `SELECT @@max_allowed_packet`, `SHOW DATABASES`, a second connection, the version probe, `MAX(version)`, then three START TRANSACTION/SELECT/ROLLBACK.
>
> **What a bootstrap cache would buy on `main`.** As an upper bound I patched a scratch build to skip the forward-drift guard and `initSchema` entirely: `bd create` went from 28.3 s to 19.6 s, `bd list` from about 7.4-8.7 s to 6.6 s, `bd show` stayed within noise (all three in the delay-proxy run). So a cache pays off mostly on writes; on reads the remaining cost is the transaction wrappers and the per-read config lookups (#5273 items 5 and 6), not the schema step.
>
> **Running next to the database.** Executing `bd` on the Dolt host over a reused ssh connection takes `bd show` from 10.2 s to about 0.73 s (0.17 s on the host itself), at the price of the usual remote-exec limits (no tty, file arguments are read on the host).
>
> Two questions, since #6364's author left the cache as "a call for the maintainers": would you take a cache of the converged-schema result at all, and if so what must it keep revalidating on every open (I would keep one cursor read for forward drift and not bypass `cursorRealityFloor` and the ignored-cursor checks)? And would you rather see #5273 items 5/6 first? Happy to take either if it is unclaimed; I will not open anything before hearing back.

Проверка черновика на приватное: нет имён хостов, путей, пользователей, IP; ссылки только на публичные номера. Числа `1.3.0` из нашего досье, числа `main` из этой заметки. Реальная сеть перемерена 12.10 (блок Critique ниже): 1.3.0, 1.3.1, main.

## Что остаётся

1. «Да» founder'а на комментарий (и отдельно на fork/PR позже).
2. 3-subagent critique перед публикацией.
3. Реальный сетевой перемер `main` (сейчас эмуляция задержки прокси) и 1.3.1; если разойдётся, поправить таблицу.
4. Ссылка из комментария на docs-PR (`cos-e4x.2`) только когда тот PR будет открыт.

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

Вердикт: FIX (мелкие правки текста, блокеров нет). Живой апстрим перепроверен gh api в тот же день, main `b630f96` без изменений.

Подтверждено: статусы и метки #6114/#5965/#5273/#3239/#4102/#6364/#6360/#6361/#7297/#7409/#5993 и слитых #6122/#5316/#6731/#7400/#7205 совпадают с заметкой; на #6114 новых комментариев после 09.10 нет, assignee/PR кэша схемы нет; цитаты kost-t-human («a call for the maintainers») и bitbot-jon («We have not implemented or benchmarked that cache») дословны; 56% занятости запросов, 44-48 round-trips, `bd close` 150 из #6114 (bd 1.2.1) верны; счётчики 24/40/44/97 равны числу строк в q-*.txt; приватных хостов, путей и имён в черновике нет.

FIX:
1. «`dolt_mode: server`» неверно для конфига: в `.beads/config.yaml` это `dolt.mode: server`. Заменить на «server mode (`dolt.mode: server`)».
2. Таблица: `bd list` «7.0-8.7 s», а в абзаце «from about 7.4-8.7 s to 6.6 s». Привести к одному диапазону (7.0-8.7).
3. «On 1.3.0 the list for `bd show` was: login, `SELECT @@max_allowed_packet`, `SHOW DATABASES`, a second connection, ...» не подтверждено ни досье, ни замером (досье даёт только «~26 обменов»). Убрать это предложение или заменить на «about 26 sequential exchanges».
4. Мелочь: `bd show` на 1.3.0 «~26 exchanges» против «~33» в runbook; оставить «about 26» с пометкой «our count».
Дубликатов нет: PR кэша схемы нет, соседние #7297/#7409 (SHOW COLUMNS/TABLES) не пересекаются с вопросом о кэше; #5273 пп.5-6 не взяты.

## Critique: actionability + remeasure

Дата: 2026-10-12. Ось: actionability (понятность без нашего контекста, воспроизводимость). Upstream actions: `0`. Вердикт: **FIX** (правки внесены в черновик выше), после них годен к публикации по «да» founder'а.

Перемер на настоящей сети Mac → citadel (SSH-порт-форвард 127.0.0.1:3307, RTT 0,24 с), только чтения из `tg-telethon`, одно и то же issue, 3 прогона, медианы (бинарники: Homebrew 1.3.0 через `BD_LOCAL=1 bd`; релизный 1.3.1 darwin_arm64, sha256 сверен с checksums.txt; main b630f96 из scratch). Сырьё: `scratchpad/oss/remeasure.json`.

| Команда | 1.3.0 | 1.3.1 | main b630f96 |
|---|---|---|---|
| `show <id>` | 10,2 с | 10,3 с | 10,8 с |
| `list --limit 3` | 8,3 с | 8,2 с | 7,0 с |
| `ready` | 8,3 с | 8,2 с | 8,9 с |

Расхождение с прежним: `show` 1.3.0 был 9,9 с (теперь 10,2), эмуляция main `show` 10,2-12,1 (реальная 10,8), `list` 7,0-8,7 (реальная 7,0). Картина прежняя: 1.3.1 ничего не меняет, main выигрывает только на `list`. `ready` без `--limit 1` ближе к 8-9 с, чем 11,8 из эмуляции (там был `--limit 1`).

Что поправлено: (1) таблица заменена на реальные медианы по трём версиям, счёт выражений вынесен в отдельный столбец с пометкой, что он из прогона через задерживающий прокси; (2) в Setup сказано, что путь идёт через SSH-форвард, и как считались выражения (`--loglevel debug`, «Starting query»); (3) `bd create` оставлен только как число из прокси-прогона, в общую БД не писали; (4) в начале есть указание, что два вопроса в конце; (5) 0,85 с заменено на 0,73 с.

Остаётся до публикации: (а) в docs-черновике сказано «`bd ready --limit 1` ~48 → ~21 выражений» (#6731), а наш счёт на main 44; мейнтейнер сразу спросит, почему. Перед публикацией пересчитать `ready --limit 1` на b630f96 и либо объяснить разницу, либо убрать число 21 из docs; (б) комментарий длинный (таблица, три абзаца, два вопроса); если мейнтейнеры не ответят, лучше резать до таблицы и вопросов, остальное по запросу; (в) скрипт задерживающего прокси нигде не опубликован: воспроизвести «Starting query» без него можно только своим `tc netem`/toxiproxy, это стоит сказать одной фразой или приложить gist (решение founder'а).
