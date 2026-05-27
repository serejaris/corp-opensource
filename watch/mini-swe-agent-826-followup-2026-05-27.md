# SWE-agent mini #826 follow-up, 2026-05-27

Live state: parent `gh` pass на 2026-05-27 19:15-19:29 UTC.

Upstream: [`SWE-agent/mini-swe-agent#826`](https://github.com/SWE-agent/mini-swe-agent/issues/826).
Internal tracker: [`corp-opensource#53`](https://github.com/serejaris/corp-opensource/issues/53).
Umbrella tracker: [`corp-opensource#52`](https://github.com/serejaris/corp-opensource/issues/52).

Upstream actions в этом блоке: 0 PR, 0 comments.

Final `next_status`: `WATCH`.

Это bounded follow-up к cycle 27. Цель была не открыть PR, а перепроверить старые blockers: live issue state, duplicate/PR search после прежнего GitHub Search rate limit, relationship to merged timeout PR `#832`, contribution/test surface, runner gate и regression-card readiness.

Required skills `open-source-bug-scouting` и `open-source-pr-workflow` в текущем session skills list недоступны. Использован documented fallback: 6 read-only scouting subagents, parent live gates через GitHub CLI/API, затем 3-subagent critique.

## Parent Live Gates

| Gate | Result |
|---|---|
| Repo state | `SWE-agent/mini-swe-agent`: 4617 stars, pushed `2026-05-25T21:55:36Z`, default branch `main`, latest release `v2.3.0`, 17 open issues, 9 open PRs in `gh repo view` snapshot. |
| Primary issue | `#826` open, label `bug`, no assignee, no comments, created `2026-05-14`, updated `2026-05-14`. |
| Source state | `src/minisweagent/environments/local.py` still uses `subprocess.run(..., shell=True, timeout=...)` in `LocalEnvironment.execute()` on `main`; no process-group/session cleanup is visible in that method. |
| Targeted duplicate search | `gh search prs/issues` for `#826`, `Agent-spawned scripts`, `start_new_session`, `killpg`, `orphan`, `LocalEnvironment timeout`, and `subprocess.run timeout` returned no exact competing PR/issue in this pass. |
| Adjacent timeout work | `#832` is merged and closes the `#803` container/agent wall-clock timeout lane, touching `src/minisweagent/agents/default.py`, `src/minisweagent/exceptions.py`, and `tests/agents/test_default.py`; it does not touch `src/minisweagent/environments/local.py`. Treat as overlap risk, not as proof that `#826` is fixed. |
| Contribution docs | `docs/contributing.md` exists. Dev setup is `pip install -e '.[dev]'`; tests use `pytest -n auto`; CI runs pytest with coverage and pylint errors-only. PR template asks for an issue reference such as `Fixes #826`. |
| Runner gate | Not satisfied. `corp-opensource#10` says no live `corp-opensource-runner` is provisioned yet; planned slot is CT `221`, still behind live-change approval. |

## 6-Subagent Synthesis

| Role | Finding |
|---|---|
| repo-fit | Strong Paperclip-like terminal-agent/harness fit: compact local execution runtime, active repo, and the issue is directly about agent-spawned command cleanup. |
| duplicate / competing PR | No exact PR duplicate found in the refreshed search. Adjacent `#832/#803/#807` are container or agent wall-clock timeout lanes, not local shell grandchild cleanup, but they must be mentioned as overlap risk. |
| contribution / process | Contribution surface is straightforward: focused Python test and local environment code, no visible CLA gate, MIT license, existing tests under `tests/environments/test_local.py`. |
| regression / repro | Existing `test_local_environment_timeout` checks structured timeout output only. A useful regression must create a shell-launched child process, record its PID, trigger `LocalEnvironment(timeout=1)`, and assert the child no longer lives after timeout, with cleanup if the pre-fix test fails. |
| patchability | Likely patch surface is narrow: `src/minisweagent/environments/local.py` plus `tests/environments/test_local.py`. A naive `start_new_session=True` on `subprocess.run()` is probably insufficient; robust POSIX behavior likely needs `Popen(..., start_new_session=True)` and timeout cleanup via process group kill, with non-POSIX fallback. |
| PR-readiness | Not PR-ready. The old duplicate-search blocker improved, but current-main runner repro, regression card, and process-tree behavior proof remain missing. |

## 3-Subagent Critique

| Role | Result |
|---|---|
| factology / duplicates | Internal `CANDIDATE` wording is defensible only with caveats, but do not claim duplicate-clean, confirmed current-main repro, or root-cause proof. `#832/#803` are adjacent timeout fixes and must stay visible. |
| process gates | Choose `WATCH`: runner gate is not closed, regression card is incomplete, and repo rules do not allow upstream comment/PR without runner-backed facts or maintainer signal. |
| actionability | Internal tracker/watch update is valid. Upstream comment now would add noise because there is no new runner-verified fact. PR is premature. |

Final decision: keep `#826` as the best mini-swe-agent lane, but final `next_status` for this bounded follow-up is exactly `WATCH`.

## Regression Card Draft

This is a draft, not a completed card.

| Field | Draft |
|---|---|
| Contract | A timeout in `LocalEnvironment.execute()` must cancel the command process tree, not only the immediate shell process. |
| Test file | `tests/environments/test_local.py`. |
| Candidate command | `python -m pytest tests/environments/test_local.py -q`; before PR, also run relevant agent timeout tests in `tests/agents/test_default.py`. |
| Pre-fix fail | Missing. Must be captured in `corp-opensource-runner` on fresh `SWE-agent/mini-swe-agent@main` after `#832`. |
| Post-fix pass | Missing. Expected: timeout returns the same structured `TimeoutExpired` result and the recorded child PID is gone. |
| Cleanup rule | The regression test must kill the recorded PID in `finally` if the pre-fix behavior leaks it. |

## Next Gate

1. Wait for or provision `corp-opensource-runner`; do not use CT `216` for this general open-source repro.
2. Clone fresh `SWE-agent/mini-swe-agent@main` in the runner.
3. Run secret-free repro for `#826` after merged `#832`.
4. Fill the regression card with actual pre-fix fail evidence and exact command.
5. Repeat duplicate PR/issue search, then run 3-subagent critique again before any upstream comment or PR.
