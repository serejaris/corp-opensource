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

