# AI-native frameworks scouting — cycle 10

Дата: 2026-05-27

Метод: 6 субагентов — repo-fit, bug-signal, repro-path, patchability, duplicate-race, PR-readiness.

Внутренний issue: [corp-opensource#39](https://github.com/serejaris/corp-opensource/issues/39)

## Итог

| Кандидат | Upstream | Решение | Почему |
|---|---|---|---|
| LiteLLM guardrail logging | [BerriAI/litellm#28971](https://github.com/BerriAI/litellm/issues/28971), [PR #28970](https://github.com/BerriAI/litellm/pull/28970) | `WATCH / duplicate-triage` | Изначально выглядел как лучший PR-ready bug, но fresh duplicate gate нашёл открытый mergeable PR #28970 по близкому guardrail logging path с regression tests и зелёными guardrails/proxy checks. Нужен compare, не новый PR. |
| E2B multi-source COPY | [e2b-dev/E2B#1349](https://github.com/e2b-dev/E2B/issues/1349) | `CANDIDATE / needs local repro` | Открытый issue, linked PR нет, баг маленький и вероятно тестируется локально через Dockerfile parser/build fixture. Следующий PR-кандидат после repro. |
| OpenHands global skills | [OpenHands/OpenHands#14563](https://github.com/OpenHands/OpenHands/issues/14563) | `COMMENT-FIRST / maintainer-direction-needed` | Баг реальный и важный для skills/runtime, но мы уже спросили upstream, хотят ли narrow CLI/self-hosted mount fix или ждать Settings/personal repo path. Без ответа PR не открывать. |
| MCP Python OAuth refresh | [modelcontextprotocol/python-sdk#2578](https://github.com/modelcontextprotocol/python-sdk/issues/2578) | `CANDIDATE / rules-gated` | Хороший no-secret unit repro, но repo в целом maintainer-gated; сначала проверить rules/current duplicates и спросить, если нет `ready for work`. |
| MCP TypeScript protocol mismatch | [modelcontextprotocol/typescript-sdk#2108](https://github.com/modelcontextprotocol/typescript-sdk/issues/2108) | `CANDIDATE / duplicate-check-needed` | Хороший wire-contract test surface; нужен fresh linked-PR/timeline check перед checkout. |
| Cline long retry-after | [cline/cline#10139](https://github.com/cline/cline/issues/10139) | `CANDIDATE / maintainer-signal-check` | Patchable retry policy bug, но надо проверить Linear/assignee/duplicate PR перед кодом. |

## Fresh duplicate checks

- `litellm#28971`: issue open, no linked PR in `closedByPullRequestsReferences`; text PR search found [#28970](https://github.com/BerriAI/litellm/pull/28970), open/mergeable, touching `guardrail_endpoints.py`, `common_request_processing.py`, proxy/enterprise guardrail tests. PR body claims guardrail logging pipeline coverage. CI mostly green in relevant guardrails/proxy lanes, with unrelated/large matrix failures still visible.
- `E2B#1349`: issue open, only Linear link comment; PR search for `1349 OR fromDockerfile OR multi-source COPY` found no exact covering PR.
- `OpenHands#14563`: issue open with maintainer root-cause comment; no open PR from search for `14563 OR global skills OR user skills openhands serve mount`.

## Урок по тестам

Главный урок цикла: **regression-first начинается до checkout, но после fresh duplicate gate**.

Неправильный порядок:

1. выбрать красивый issue;
2. написать тест;
3. открыть PR;
4. потом обнаружить, что рядом уже есть покрывающий PR.

Правильный порядок:

1. проверить issue timeline, linked/cross-referenced PRs и text PR search;
2. если чисто — записать regression card в internal issue/watch note;
3. написать тест, который падает на pre-fix state или на reverted fix;
4. только потом делать минимальный patch и PR.

`pydantic-ai#5679` и `litellm#28971` оба показывают одну ловушку: кандидат может быть технически хорошим, но уже занятым. В таком случае полезная работа — сравнить тесты/поведение и, если есть конкретный gap, принести его в существующий PR или issue. Новый PR без gap = шум.

## Следующий action

1. Для `litellm#28971` сравнить, закрывает ли #28970 именно `/v1/responses` + `LiteLLM_SpendLogs.guardrail_information`, а не только `/apply_guardrail` callbacks.
2. Если #28970 не покрывает Responses spend log path, comment-first с конкретным missing regression:
   - `contract`: blocked `/v1/responses` guardrail call persists `guardrail_information` into `LiteLLM_SpendLogs`;
   - `test file`: `tests/test_litellm/proxy/guardrails/...` или ближайший existing proxy spend log test;
   - `command`: smallest guardrail/proxy pytest selection;
   - `pre-fix fail`: spend log row has empty/null `guardrail_information`;
   - `post-fix pass`: spend log contains the guardrail block metadata used by Guardrails Monitor.
3. Если #28970 уже покрывает этот contract, оставить `WATCH / duplicate-covered`.
4. Если LiteLLM закрыт, следующий clean candidate — `E2B#1349` с local parser/build fixture.
