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
3. **Patchability**: вероятные файлы, fixtures, targeted tests, риск большого refactor.

## Выход

Для каждого кандидата создать issue:

- upstream repo + ссылки;
- score;
- состояние repro;
- patch surface;
- следующий конкретный шаг.

