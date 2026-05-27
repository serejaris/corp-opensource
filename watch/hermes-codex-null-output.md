# Hermes Codex Null Output Watch

## Outcome

- Our PR: https://github.com/NousResearch/hermes-agent/pull/32884
- Created first: `2026-05-27T00:04:42Z`
- Closed as duplicate: `2026-05-27T02:38:29Z`
- Upstream fix landed through: https://github.com/NousResearch/hermes-agent/pull/32963
- Merged at: `2026-05-27T02:37:37Z`
- Competing broader PR: https://github.com/NousResearch/hermes-agent/pull/32890

## Why upstream selected another PR

Our PR covered:

- `agent/codex_runtime.py`
- `tests/run_agent/test_run_agent_codex_responses.py`

The competing PR also covered:

- `agent/auxiliary_client.py`
- `tests/agent/test_auxiliary_client.py`

Maintainer selected the broader path via #32963. The useful process lesson is not “first PR wins”; it is “first PR must actively consolidate duplicate coverage.”

## Signals

- PR #32884 received 18 reactions by the last check.
- A user confirmed the patch restored two Hermes installs.
- Contributor `iqdoctor` opened the related OpenAI Python SDK follow-up: https://github.com/openai/openai-python/pull/3315
- `teknium1` closed #32884 as duplicate after #32963 merged.

## Follow-up

- Watch for a Hermes release containing merge commit `2a8d2174173ab8d05d0b48a44580a8c0b2c8c19b`.
- Watch openai-python PR #3315.
- Keep local skill updated with duplicate triage and subagent rules.

## SDK vs Hermes boundary - 2026-05-27

Чат-сигнал и subagent triage показали важное разделение:

- `openai-python` root-layer bug: SDK parser делает `for output in response.output` и падает, если terminal Responses object пришёл с `output=None` или missing `output`.
- Hermes #32963: downstream recovery/workaround, а не SDK one-liner. Он восстанавливает output из уже полученных stream events в `agent/codex_runtime.py` и `agent/auxiliary_client.py`.
- `openai/openai-python#3315`: upstream SDK cleanup, сейчас open; там уже есть не только `response.output or []`, но и stream-state preservation for terminal `output=None/[]`.

Оставшийся риск в Hermes #32963:

- main path ловит `TypeError` по строковому признаку `"NoneType"` + `"not iterable"`;
- `except TypeError` стоит вокруг широкого блока stream handling;
- если Hermes callback/event-processing код сам выбросит `TypeError: 'NoneType' object is not iterable` после уже полученных deltas/items, recovery может ошибочно вернуть partial response и скрыть локальный баг.

Нужный follow-up:

- сузить scope `except TypeError` до SDK stream iteration / final parser failure;
- добавить regression: Hermes-side callback/handler `TypeError("NoneType ... not iterable")` должен пробрасываться, а не маскироваться recovery;
- не спорить с #32963 как outage fix: он реально закрыл #11179.

