# oh-my-pi #12870: Opus 5.5 отклоняется как `claude_code_version_too_old`, 2026-09-22

Upstream: [can1357/oh-my-pi#12870](https://github.com/can1357/oh-my-pi/issues/12870), тот же root cause что и [#10494](https://github.com/can1357/oh-my-pi/issues/10494).

## Симптом

omp `18.2.8` на Anthropic OAuth-подписке шлёт Claude Code fingerprint `2.1.257` (User-Agent и `x-anthropic-billing-header: cc_version=2.1.257.<sha3>` в `system[0]`). Anthropic пускает `claude-opus-5-5` только с `2.1.280+`, поэтому любой запрос к модели получает 400 `invalid_request_error` / `claude_code_version_too_old` ещё до обращения к модели.

## Parent live gates

Состояние на `2026-09-22 ~19:30 UTC`:

- Issue open, labels `bug`, `duplicate`, `providers`, `provider:anthropic`, `triaged`; assignee нет.
- Fix уже на `main` и в релизе:
  - `0a4dfe1a` поднимает `DEFAULT_CLAUDE_CODE_VERSION` в `packages/ai/src/providers/claude-code-fingerprint.ts` до `2.1.280`;
  - `228222665` добавляет `adoptRequiredClaudeCodeVersion()`: из ответа `claude_code_version_too_old` берётся требуемая версия, `streamAnthropicOnce` пересобирает client и params (включая billing header) и делает одну повторную попытку. Следующий устаревший пин будет стоить одного отклонённого запроса, а не полного отказа.
- Tag `v18.2.9` = `main` HEAD `8cd6f8c61`. GitHub release опубликован `18:41Z`, npm `@oh-my-pi/pi-ai@18.2.9` — `18:52Z` (`latest`), в опубликованном `src/providers/claude-code-fingerprint.ts` стоит `2.1.280`.
- Открытых PR с exact cover нет, и он не нужен: fix принадлежит maintainer'у и уже выпущен.

## Follow-up patrickab (stale fingerprint на 18.2.9)

Комментарий: на `omp --version` = `18.2.9` в логе 400 всё ещё `cc_version=2.1.257.9c3`.

Source-проверка v18.2.9:

- `git grep 2.1.257` находит только тестовую фикстуру; других источников версии в src нет.
- `getClaudeCodeVersion()` = `PI_AI_CLAUDE_CODE_VERSION` → adopted → `DEFAULT` (`2.1.280`). Adoption версию только повышает.
- Старая версия в 18.2.9 может появиться только тремя путями: (1) env `PI_AI_CLAUDE_CODE_VERSION`, в том числе из `~/.env`, `~/.omp/.env`, agent `.env` или project `.env`, которые omp подгружает сам; (2) system prompt уже содержит блок `x-anthropic-billing-header:` — `buildAnthropicSystemBlocks` тогда свой header не генерирует и пересылает чужой (путь auth-gateway `/v1/messages`); (3) запрос ушёл от другого, старого процесса или бинарника (например, `~/.bun/bin/omp` раньше в PATH или сессия, запущенная до апгрейда).
- `http-400-requests` логи не пишут версию omp, поэтому по логу репортера процесс не атрибутировать.

Вывод: code bug в 18.2.9 не подтверждён; наиболее вероятна причина (3). Upstream comment не делали: изменения статуса кандидата нет, 3-role critique не требуется.

## Локальный fix и проверка (машина владельца)

- До: `~/.bun/bin/omp` = `omp/18.2.8`. В `~/.omp/logs/http-400-requests` есть свежие отказы `claude-opus-5-5` с `cc_version=2.1.257.407` / `.efd` и `version 2.1.280 or newer is required`.
- С approval владельца выполнен `omp update`: скачан `omp-darwin-arm64` v18.2.9, `Verified sha256:42f0e972a355079aab7a35f2a74fe7bf3b46d500d44aa876770ad2b8a1f5506e` (совпадает с digest ассета релиза).
- После: `omp/18.2.9`; в бинарнике есть строка `"2.1.280"`, строки `2.1.257` нет.
- Live smoke: `omp -p --model anthropic/claude-opus-5-5 "Reply with exactly the single word: ok"` → `ok`, exit `0`; session log `model: claude-opus-5-5`, `stopReason: stop`; новых `http-400-requests` нет (51 → 51).
- Запущенных omp-процессов на момент апгрейда не было, так что перезапускать ничего не требовалось.

## Decision

`next_status: NO-GO`: upstream уже исправил и выпустил. Upstream actions: `0`, runner actions: `0` (runner не нужен: repro и проверка — живой запрос на машине владельца).

Если follow-up patrickab подтвердится на чистом 18.2.9 (один бинарник в PATH, без `PI_AI_CLAUDE_CODE_VERSION`), возможная дорожка — путь (2): client-supplied billing header в `buildAnthropicSystemBlocks` отключает adoption/retry. Статус тогда `LEAD`, и нужны repro через auth-gateway и duplicate search.

## Урок

При `claude_code_version_too_old` сначала сверь `omp --version` и `which -a omp` с версией релиза, где поднят пин, и только потом ищи code bug. Устаревший процесс или второй бинарник в PATH выглядит в логах точно так же, как устаревший пин.
