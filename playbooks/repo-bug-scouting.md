# Разведка багов в репозиториях

Использовать, когда нужно найти следующий open-source contribution target.

## Вход

- дорожка: AI agents, AI runtime, conversion tools, self-hosted products;
- окно активности: repo updated за 7 дней, issues за 30 дней;
- минимальный сигнал: баг воспроизводим или есть достаточно данных для repro.

## Команды

```bash
gh search repos --language Python --stars '>50000' --sort stars --limit 20 --json fullName,description,stargazersCount,updatedAt,url
gh search repos --language TypeScript --stars '>50000' --sort stars --limit 20 --json fullName,description,stargazersCount,updatedAt,url

gh search issues 'repo:OWNER/REPO is:issue is:open label:bug updated:>=YYYY-MM-DD' --limit 30
gh search issues 'repo:OWNER/REPO is:issue is:open regression OR crash OR TypeError OR broken' --limit 30
gh search prs 'repo:OWNER/REPO is:pr is:open bug OR regression OR crash' --limit 30
```

## Субагенты

Запускать параллельно на этапе scouting/triage:

1. **Repo-fit**: contribution guide, test commands, recent merged PR, maintainer response.
2. **Bug-signal**: свежие баги, реакции, дубликаты, user impact.
3. **Repro-path**: минимальный script, fixture, CLI command, Docker или failing test.
4. **Patchability**: вероятные файлы, fixtures, targeted tests, риск большого refactor.
5. **Duplicate-race**: открытые/смерженные конкурирующие PR, linked issues, похожие fixes.
6. **PR-readiness**: CONTRIBUTING, PR template, fork workflow, required checks, draft-vs-ready.

После синтеза родитель обязан сделать live gates сам: issue state, assignee/labels, linked PRs/timeline, PR search, contribution docs, regression card.

Перед upstream comment/PR запускать отдельный 3-subagent critique:

1. **Fact-check critique**: ловит оверклеймы про freshness, maintainer signal, duplicate coverage, CI/mergeability и `subagent GO`.
2. **Process-gate critique**: проверяет, что порядок не нарушен: `6-subagent scouting -> parent live gates -> 3-subagent critique -> comment/PR/tracker update`.
3. **Actionability critique**: требует один конкретный `next_status`: `LEAD`, `CANDIDATE`, `COMMENT-FIRST`, `PR-READY`, `PR-OPEN`, `WATCH`, `NO-GO`; для `PR-READY` нужен regression-first contract, для `COMMENT-FIRST` — точный вопрос/предложение, для `WATCH` — событие-разблокировщик.

## Allowed next_status

- `LEAD`: только subagent-сигнал; parent live gates ещё не пройдены. Нельзя комментировать upstream или писать код.
- `CANDIDATE`: parent live gates чистые, есть regression card, но pre-fix fail ещё не доказан. Это не `PR-READY`.
- `COMMENT-FIRST`: баг вероятен, но repo/social gate требует сначала вопрос, assignment, maintainer direction или confirmation.
- `PR-READY`: fresh duplicate gate чистый, contribution gate разрешает PR, regression card заполнена, pre-fix fail доказан, patch surface минимален, verification command известна.
- `PR-OPEN`: PR открыт; дальше только follow-up loop.
- `WATCH`: полезно наблюдать, но сейчас нет разрешения на PR: `needs-repro`, `needs-reporter-path`, `maintainer-direction-needed`, `assignment-first`, `duplicate-race`, `release-followup`.
- `NO-GO`: не открывать PR в этом цикле: `duplicate-covered`, assignment-gated with closed attempts, invitation-only without invite, upstream main already fixed, no public patch surface.

## After critique decision gate

После 3-subagent critique parent обязан выбрать ровно один статус:

- `PR-READY`, если duplicate gate clean, contribution gate разрешает PR, pre-fix failure доказан, test file + command известны, patch touches minimal public boundary.
- `COMMENT-FIRST`, если duplicate gate clean, но repo просит comment-before-code; нет assignment/ready label; maintainer обсуждает product direction; patch затрагивает public API/runtime semantics; есть exact regression card, которую можно предложить.
- `WATCH`, если баг не воспроизведён; upstream main не ломается на проверенном path; нужен reporter version/body/log; runner недоступен; есть active same-repo PR by us waiting review; есть competing PR, но coverage ещё не сравнён.
- `NO-GO`, если existing PR covers same contract + test; issue assignment-gated and previous drive-by PRs were closed; repo is invitation-only without invite; upstream removed affected code path; no public contribution surface.

## Hard PR gate

Перед `gh pr create` все пункты должны быть true:

- watch note / internal issue updated after critique;
- current issue state, assignee, labels and timeline checked;
- linked/cross-referenced PRs checked, not only text search;
- contribution gate says PR is allowed now;
- no active same-repo PR by us is waiting review/eval unless this issue is clearly independent and urgent;
- pre-fix or reverted-fix failure recorded with exact assertion/error;
- targeted post-fix command passed;
- required owner-level test/typecheck/format command identified or explicitly marked not applicable;
- upstream comment posted first if repo requires comment-before-code.

## WATCH recheck trigger

Каждый `WATCH` должен иметь `unblock_event`: `maintainer-reply`, `assignment/ready label`, `reporter provides version/path/body`, `runner available`, `competing PR merged/closed`, `release published`, `our active same-repo PR reviewed/merged/closed`, or `scheduled recheck date`.

Без `unblock_event` статус считается не actionable и должен стать `NO-GO` или закрытым stale internal candidate.

## Выход

Для каждого кандидата создать issue:

- upstream repo + ссылки;
- score;
- состояние repro;
- patch surface;
- следующий конкретный шаг.

## Урок по тестам

Для bugfix PR сначала ищем контракт, который реально сломан у пользователя, и пишем/проверяем regression test на этом контракте.

Порядок обязателен:

1. fresh duplicate gate: issue timeline, linked/cross-referenced PRs, text PR search;
2. regression card в issue/watch note;
3. 3-subagent critique по фактологии, process gates и actionability;
4. pre-fix failing test или reverted-fix failing test;
5. минимальный patch;
6. повторный duplicate gate прямо перед `gh pr create`.

Нельзя считать PR готовым, если:

- есть только helper-level unit test, а пользовательский путь не покрыт;
- test был запущен только после фикса и не показал pre-fix failure;
- reference PR существует, но мы не сравнили его тесты и не поняли, какой контракт он доказывает;
- fresh duplicate gate нашёл открытый PR, но мы не сравнили, покрывает ли он наш user-facing contract;
- upstream `main` уже выглядит исправленным, но мы всё равно начинаем ветку без duplicate triage.
- critique не зафиксирована в issue/watch note для свежего scouting candidate.

Минимальный стандарт: targeted test падает на текущем сломанном состоянии или на reverted fix, затем проходит после patch. Если это дорого, фиксируем `WATCH / needs-repro`, а не открываем PR.

Перед кодом в issue/watch note должна быть короткая карточка:

- `контракт`: что ломается у пользователя;
- `test file`: куда пойдёт regression;
- `command`: самая маленькая команда проверки;
- `pre-fix fail`: ожидаемый assertion/error;
- `post-fix pass`: какая команда должна пройти после patch.

Для container/runtime visibility багов тест должен стоять на границе, где ломается пользовательский контракт: Docker volumes, env passthrough, CLI command generation, request payload или wire protocol. Helper-level тест на parser/loader недостаточен, если реальный баг в том, что helper не получает нужный filesystem/path/env внутри container.

Для browser-agent action/schema багов тест должен начинаться с raw model/tool payload, который реально пришёл в систему. Если модель прислала `{"input_text": {"index": true}}`, regression должен проверить validation/normalization/execution boundary: reject или structured warning, сохранение `raw_action`, `validated_action`, и невозможность молча выполнить действие по элементу `1` без trace. Helper-level pydantic/zod test полезен только после boundary test.

Для streaming adapter bugs synthetic event histories недостаточны, если maintainer просит real traffic evidence. Regression должен доказывать wire stream shape через VCR/cassette или recorded provider chunks: какие `output_item.added/done/completed` события реально пришли, и где именно adapter backfill нужен. Hand-crafted sequence годится только как дополнительный unit test.

Для downstream recovery вокруг SDK/dependency parser failures тестировать надо не строковый predicate, а границу ответственности. Урок Hermes/Codex #32999: если recovery ловит `TypeError("'NoneType' object is not iterable")`, regression обязан доказать:

- SDK/dependency failure на публичном stream/runtime path действительно recover;
- app-side callback/handler/post-processing `TypeError` с тем же class/message пробрасывается наружу;
- тест падает на старом широком `try/except`, потому что именно placement recovery-блока был багом;
- если upstream переписал архитектуру и больше не ходит через сломанный SDK helper, PR можно закрыть, но boundary-regression lesson остаётся в playbook/skill.

Для SSE/wire-framing багов не тестировать внешний timeout как основной контракт, если timeout живёт в клиенте или UI. Сначала выделить deterministic SDK boundary: какие байты/строки транспорт пишет в wire. Пример: U+2028/U+2029 валидны внутри JSON string, но не должны выходить literal внутри одной SSE `data:` line; regression должен проверять raw frame (`data:` содержит `\\u2028`/`\\u2029`) и round-trip через `JSON.parse`, а не ждать 300 секунд Claude/UI timeout.

Для optional dependency import bugs тест должен начинаться с публичного import boundary: импортируем тот module/shim, который ломается у пользователя, и проверяем понятный install-extra hint. Пример: для `google/adk-python#5864` regression временно подменяет `sys.modules["vertexai"] = None`, импортирует `google.adk.dependencies.vertexai` и требует `google-adk[gcp]` / `google-adk[all]` в ошибке. Helper-only тест здесь не доказывает, что пользователь перестал видеть raw missing-module failure.

Для invitation-only репозиториев отдельный gate: без явного приглашения не открывать upstream PR, даже если fix очевиден. В таком случае полезная работа — watch note, issue, duplicate/test analysis и короткий upstream comment только при наличии нового факта.

Если candidate стал duplicate во время разведки, не считать работу потерянной. Сохранить test contract и сравнить с существующим PR:

- если существующий PR покрывает contract и тест — `WATCH / duplicate-covered`;
- если покрывает patch, но не тест — предложить missing regression в issue/PR;
- если покрывает соседний path, но не reported path — comment-first с точной regression card.
