# AI-native frameworks scouting — cycle 10

Дата: 2026-05-27

Метод: 6 субагентов — repo-fit, bug-signal, repro-path, patchability, duplicate-race, PR-readiness.

Внутренний issue: [corp-opensource#39](https://github.com/serejaris/corp-opensource/issues/39)

## Итог

| Кандидат | Upstream | Решение | Почему |
|---|---|---|---|
| LiteLLM guardrail logging | [BerriAI/litellm#28971](https://github.com/BerriAI/litellm/issues/28971), [PR #28970](https://github.com/BerriAI/litellm/pull/28970) | `WATCH / duplicate-triage` | Изначально выглядел как лучший PR-ready bug, но fresh duplicate gate нашёл открытый mergeable PR #28970 по близкому guardrail logging path с regression tests и зелёными guardrails/proxy checks. Нужен compare, не новый PR. |
| E2B multi-source COPY | [e2b-dev/E2B#1349](https://github.com/e2b-dev/E2B/issues/1349), [PR #1354](https://github.com/e2b-dev/E2B/pull/1354) | `PR OPEN / CLA-gated` | Red/green regression proved locally; PR opened ready. External blocker: `verification/cla-signed` requires @serejaris CLA signature. |
| OpenHands global skills | [OpenHands/OpenHands#14563](https://github.com/OpenHands/OpenHands/issues/14563) | `COMMENT-FIRST / maintainer-direction-needed` | Баг реальный и важный для skills/runtime, но мы уже спросили upstream, хотят ли narrow CLI/self-hosted mount fix или ждать Settings/personal repo path. Без ответа PR не открывать. |
| MCP Python OAuth refresh | [modelcontextprotocol/python-sdk#2578](https://github.com/modelcontextprotocol/python-sdk/issues/2578) | `CANDIDATE / rules-gated` | Хороший no-secret unit repro, но repo в целом maintainer-gated; сначала проверить rules/current duplicates и спросить, если нет `ready for work`. |
| MCP TypeScript protocol mismatch | [modelcontextprotocol/typescript-sdk#2108](https://github.com/modelcontextprotocol/typescript-sdk/issues/2108) | `CANDIDATE / duplicate-check-needed` | Хороший wire-contract test surface; нужен fresh linked-PR/timeline check перед checkout. |
| Cline long retry-after | [cline/cline#10139](https://github.com/cline/cline/issues/10139) | `CANDIDATE / maintainer-signal-check` | Patchable retry policy bug, но надо проверить Linear/assignee/duplicate PR перед кодом. |

## Fresh duplicate checks

- `litellm#28971`: issue open, no linked PR in `closedByPullRequestsReferences`; text PR search found [#28970](https://github.com/BerriAI/litellm/pull/28970), open/mergeable, touching `guardrail_endpoints.py`, `common_request_processing.py`, proxy/enterprise guardrail tests. PR body claims guardrail logging pipeline coverage. CI mostly green in relevant guardrails/proxy lanes, with unrelated/large matrix failures still visible.
- `E2B#1349`: issue open, only Linear link comment; PR search for `1349 OR fromDockerfile OR multi-source COPY` found no exact covering PR.
- `OpenHands#14563`: issue open with maintainer root-cause comment; no open PR from search for `14563 OR global skills OR user skills openhands serve mount`.

Update after exact comparison: #28970 is adjacent, not covering. It only touches `/apply_guardrail` and tests around `guardrail_endpoints.py`; it does not touch `/v1/responses`, `response_api_endpoints`, or the failure spend-log path that writes `LiteLLM_SpendLogs.metadata.guardrail_information`. Posted upstream duplicate-check / missing-regression comment: <https://github.com/BerriAI/litellm/issues/28971#issuecomment-4553229686>.

Decision: keep `litellm#28971` as `COMMENT-FIRST / missing-regression-offered`. Do not open a PR until maintainers confirm the `/v1/responses` spend-log path is still uncovered or ask for a patch.

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

1. Ждать maintainer signal по `litellm#28971`; если green-light, PR должен начинаться с regression card:
   - `contract`: blocked `/v1/responses` guardrail call persists `guardrail_information` into `LiteLLM_SpendLogs`;
   - `test file`: `tests/test_litellm/proxy/guardrails/...` или ближайший existing proxy spend log test;
   - `command`: smallest guardrail/proxy pytest selection;
   - `pre-fix fail`: spend log row has empty/null `guardrail_information`;
   - `post-fix pass`: spend log contains the guardrail block metadata used by Guardrails Monitor.
2. Для `E2B#1354`: подписать E2B CLA для `@serejaris`, затем написать `@cla-bot check` в PR.

## E2B #1349 execution

Local checkout: `/Users/ris/Documents/GitHub/e2b-1349`

Branch: `fix-fromdockerfile-multi-source-copy`

PR: <https://github.com/e2b-dev/E2B/pull/1354>

Pre-fix regression proof:

```text
pnpm --filter e2b exec vitest run tests/template/methods/fromDockerfile.test.ts -t "multi-source COPY"
```

Result before patch: failed as expected. Assertion:

```text
expected 'USER' to equal 'COPY'
```

Meaning: `Template().fromDockerfile("COPY package.json package-lock.json /app/")` emitted only one `COPY` step, then the next instruction was the default `USER`, so the second source was dropped.

Patch:

- `packages/js-sdk/src/template/dockerfileParser.ts`: `handleCopyInstruction()` now treats every argument before the last one as a source and emits one `templateBuilder.copy(src, dest, { user })` per source.
- `packages/js-sdk/tests/template/methods/fromDockerfile.test.ts`: adds regression for multi-source `COPY`.
- `.changeset/fromdockerfile-multi-source-copy.md`: patch changeset for package `e2b`.

Post-fix verification:

```text
pnpm --filter e2b exec vitest run tests/template/methods/fromDockerfile.test.ts -t "multi-source COPY" -> 1 passed
pnpm --filter e2b exec vitest run tests/template/methods/fromDockerfile.test.ts -> 5 passed
pnpm --filter e2b run typecheck -> passed
pnpm --filter e2b run lint -> passed
pnpm --filter e2b run build -> passed
pnpm --filter e2b exec prettier --check src/template/dockerfileParser.ts tests/template/methods/fromDockerfile.test.ts -> passed
```

Initial PR state:

- Open, ready, mergeable.
- `reviewDecision=REVIEW_REQUIRED`.
- `changeset-bot`: patch changeset detected.
- `verification/cla-signed`: error; CLA needs signing at <https://e2b.dev/docs/cla>, then comment `@cla-bot check`.
- Vercel check is failure/authorization URL, likely external preview authorization rather than code failure.
