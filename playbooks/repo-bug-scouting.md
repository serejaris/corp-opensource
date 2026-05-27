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

Нельзя считать PR готовым, если:

- есть только helper-level unit test, а пользовательский путь не покрыт;
- test был запущен только после фикса и не показал pre-fix failure;
- reference PR существует, но мы не сравнили его тесты и не поняли, какой контракт он доказывает;
- upstream `main` уже выглядит исправленным, но мы всё равно начинаем ветку без duplicate triage.

Минимальный стандарт: targeted test падает на текущем сломанном состоянии или на reverted fix, затем проходит после patch. Если это дорого, фиксируем `WATCH / needs-repro`, а не открываем PR.

Для invitation-only репозиториев отдельный gate: без явного приглашения не открывать upstream PR, даже если fix очевиден. В таком случае полезная работа — watch note, issue, duplicate/test analysis и короткий upstream comment только при наличии нового факта.
