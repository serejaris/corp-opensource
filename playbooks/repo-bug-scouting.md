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

Запускать параллельно:

1. **Repo-fit**: contribution guide, test commands, recent merged PR, maintainer response.
2. **Bug-signal**: свежие баги, реакции, дубликаты, user impact.
3. **Repro-path**: минимальный script, fixture, CLI command, Docker или failing test.
4. **Patchability**: вероятные файлы, fixtures, targeted tests, риск большого refactor.
5. **Duplicate-race**: открытые/смерженные конкурирующие PR, linked issues, похожие fixes.
6. **PR-readiness**: CONTRIBUTING, PR template, fork workflow, required checks, draft-vs-ready.

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
3. pre-fix failing test или reverted-fix failing test;
4. минимальный patch;
5. повторный duplicate gate прямо перед `gh pr create`.

Нельзя считать PR готовым, если:

- есть только helper-level unit test, а пользовательский путь не покрыт;
- test был запущен только после фикса и не показал pre-fix failure;
- reference PR существует, но мы не сравнили его тесты и не поняли, какой контракт он доказывает;
- fresh duplicate gate нашёл открытый PR, но мы не сравнили, покрывает ли он наш user-facing contract;
- upstream `main` уже выглядит исправленным, но мы всё равно начинаем ветку без duplicate triage.

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

Для SSE/wire-framing багов не тестировать внешний timeout как основной контракт, если timeout живёт в клиенте или UI. Сначала выделить deterministic SDK boundary: какие байты/строки транспорт пишет в wire. Пример: U+2028/U+2029 валидны внутри JSON string, но не должны выходить literal внутри одной SSE `data:` line; regression должен проверять raw frame (`data:` содержит `\\u2028`/`\\u2029`) и round-trip через `JSON.parse`, а не ждать 300 секунд Claude/UI timeout.

Для invitation-only репозиториев отдельный gate: без явного приглашения не открывать upstream PR, даже если fix очевиден. В таком случае полезная работа — watch note, issue, duplicate/test analysis и короткий upstream comment только при наличии нового факта.

Если candidate стал duplicate во время разведки, не считать работу потерянной. Сохранить test contract и сравнить с существующим PR:

- если существующий PR покрывает contract и тест — `WATCH / duplicate-covered`;
- если покрывает patch, но не тест — предложить missing regression в issue/PR;
- если покрывает соседний path, но не reported path — comment-first с точной regression card.
