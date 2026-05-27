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

## Follow-up PR - 2026-05-27

- Upstream PR: https://github.com/NousResearch/hermes-agent/pull/32999
- Internal issue: https://github.com/serejaris/corp-opensource/issues/6
- Branch: `serejaris:codex/hermes-typeerror-hardening`
- Commit: `7f23f8b61 fix(codex): narrow null-output stream recovery`

Что изменено:

- recovery на `TypeError("NoneType ... not iterable")` перенесён внутрь SDK-boundary:
  - `next(stream_iter)`;
  - `stream.get_final_response()`.
- Hermes-side callback/body `TypeError` больше не попадает в null-output recovery.
- Добавлены regressions:
  - SDK null-output при stream iteration продолжает recover;
  - SDK null-output при `get_final_response()` продолжает recover;
  - callback `TypeError("'NoneType' object is not iterable")` пробрасывается.

Validation:

- Local syntax: `python3 -m py_compile agent/codex_runtime.py tests/run_agent/test_run_agent_codex_responses.py`
- CT216 targeted: `scripts/run_tests.sh tests/run_agent/test_run_agent_codex_responses.py -- -q -k 'null_output or callback_type_error'` -> `3 passed`
- CT216 full file: `scripts/run_tests.sh tests/run_agent/test_run_agent_codex_responses.py -- -q` -> `68 passed`

PR state after create:

- Open, ready for review, mergeable.
- Checks: no checks reported on branch at first check; keep monitoring.

## Follow-up loop check - 2026-05-27

Checked upstream state after user reminder that previous work needs periodic follow-up:

- #32963 remains merged with green checks.
- #32884 remains closed as duplicate.
- #32999 remains open, ready, mergeable, with no checks reported on branch.
- New related PR #33017 opened by `iqdoctor`: usage-preservation during null-output recovery.
- Latest check: #33017 is open/mergeable; all reported checks are green except `build-arm64`, still in progress.

Comparison:

- #33017 touches `agent/auxiliary_client.py`, `agent/codex_runtime.py`, `tests/agent/test_auxiliary_client.py`, `tests/run_agent/test_run_agent_codex_responses.py`.
- #32999 touches `agent/codex_runtime.py`, `tests/run_agent/test_run_agent_codex_responses.py`.
- #33017 preserves terminal `response.completed.response.usage`.
- #32999 keeps callback/handler `TypeError("'NoneType' object is not iterable")` from being swallowed by null-output recovery.

Action taken:

- Posted coordination comment on #32999:
  https://github.com/NousResearch/hermes-agent/pull/32999#issuecomment-4551108788
- Message: #33017 and #32999 overlap in files but cover different behavior; if #33017 lands first, rebase #32999 and keep only the distinct recovery-boundary change/test.

Next check:

- Watch whether #33017 merges before #32999.
- If #33017 lands first, rebase #32999 and rerun `tests/run_agent/test_run_agent_codex_responses.py`.
- If maintainers close #32999 as duplicate, compare whether callback-error regression is covered before accepting closure.

## Follow-up loop check - 2026-05-27 04:14Z

- #32999 remains open, ready, mergeable, with no checks reported.
- #33017 remains open and mergeable.
- #33017 checks are now complete: all substantive checks passed; only follow-on merge/move/save jobs are skipped.

Interpretation:

- No new upstream comment needed yet: the distinction between #32999 and #33017 is already documented on #32999.
- Next action is still to watch merge order. If #33017 lands first, rebase #32999 onto the merged recovery shape and preserve only the callback-error regression/hardening.

## Follow-up loop check - 2026-05-27 08:00Z

- #32999 remains open but is now `CONFLICTING`.
- #33017 remains open, also `CONFLICTING`; its substantive checks were green before conflict state.
- Additional duplicate PRs appeared around the same Codex null-output outage, including #33021, #33038, #33050, #33091, #33103 and others.
- `iqdoctor` left a support comment on #32999 confirming the narrow recovery boundary is the healthier interim shape while `openai-python#3315` handles the root SDK invariant.
- `alt-glitch` commented on #33017 that it competes with #33025 and recommended consolidation.

Interpretation:

- Do not open another Hermes PR for this outage.
- Do not treat reactions/support as maintainer approval; wait for maintainer consolidation direction or rebase only after the canonical branch/PR is clear.
- The durable value of #32999 is the boundary regression: SDK null-output errors are recoverable, but Hermes callback/local processing `TypeError("'NoneType' object is not iterable")` must propagate.

Testing lesson:

- For downstream recovery around dependency parser failures, the first regression must prove both sides of the boundary:
  - dependency/SDK failure recovers through the intended public stream path;
  - same-looking app-side callback/handler failure does not recover.
- A helper-only predicate test would have missed this bug because the failure was the placement of `except TypeError`, not the string predicate itself.

## Final follow-up for #32999 - 2026-05-27

Upstream closed #32999 as obsolete:

- Close comment: https://github.com/NousResearch/hermes-agent/pull/32999#issuecomment-4552945709
- Maintainer explanation: #33042 merged commit `cb38ce28c` removes `client.responses.stream(...)` from both Codex call sites and uses `client.responses.create(stream=True)` raw event iteration, assembling final content from `response.output_item.done`.
- Because Hermes no longer reads terminal `response.completed.output` for content reconstruction, the original SDK `output=null` TypeError path is structurally impossible in those Codex call sites.

Decision:

- Accept closure; do not reopen or argue.
- The practical outage fix landed through #32963 and later architectural replacement through #33042.
- Keep the durable lesson from #32999: recovery around dependency parser failures needs a boundary regression proving that app-side callback/local processing `TypeError("'NoneType' object is not iterable")` still propagates.

Next:

- No Hermes PR action pending for this outage.
- Continue watching `openai/openai-python#3315` as the root SDK invariant cleanup.
