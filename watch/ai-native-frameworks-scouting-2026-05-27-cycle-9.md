# AI-native Frameworks Scouting Cycle 9

Checked at `2026-05-27T09:00Z`.

## Method

Six-subagent scouting pass plus local duplicate/rules checks where needed.

## Shortlist

| Rank | Target | Decision | Why | Next action |
|---:|---|---|---|---|
| 1 | [BerriAI/litellm#28962](https://github.com/BerriAI/litellm/issues/28962) Gemini AI Studio 5xx leaks as `MaskedHTTPStatusError` | `NO-GO / release-followup` | Fresh probes show current branch already maps mocked Gemini 503 `MaskedHTTPStatusError` to `ServiceUnavailableError` through `exception_type` and public `acompletion` | Watch reporter/upstream; do not open a PR unless the exact leaking path is confirmed |
| 2 | [modelcontextprotocol/python-sdk#2687](https://github.com/modelcontextprotocol/python-sdk/issues/2687) `AnyUrl` vs `AnyHttpUrl` OAuth redirect mismatch | `COMMENT-FIRST / rules-gated` | Very patchable, but repo rules say comment before starting and ideally wait for maintainer feedback / `ready for work` | Watch/comment only unless maintainers invite PR |
| 3 | [microsoft/playwright-mcp#1635](https://github.com/microsoft/playwright-mcp/issues/1635) `browser_navigate_back` timeout after URL changed | `CANDIDATE / needs repro` | Excellent repo fit and deterministic browser/MCP surface; needs local repro and duplicate check before code | Add to future queue after current active PR load |
| 4 | [modelcontextprotocol/typescript-sdk#2155](https://github.com/modelcontextprotocol/typescript-sdk/issues/2155) `U+2028/U+2029` tool response timeout | `CANDIDATE / needs repro matrix` | Strong wire/protocol bug signal, no direct PR found by subagent | Repro matrix before PR |
| 5 | [cline/cline#9848](https://github.com/cline/cline/issues/9848) raw XML tool invocation leaks into chat | `COMMENT-FIRST` | High agent-runtime impact, but maintainer already said code location identified and another contributor asked to take it | Wait/polite comment only with concrete test plan |

## Regression-first Lesson

The reusable lesson from the Agno duplicate and this scouting cycle: a test is not enough if it only exists after the patch.

Before a bugfix PR, the internal issue/watch note must include:

- user-facing contract;
- exact regression test file;
- exact targeted command;
- expected pre-fix assertion/error;
- post-fix command/result.

If that pre-fix failure is not available cheaply, the candidate is `WATCH / needs-repro`, not `GO`.

## Duplicate Gate

Use issue timeline/linked PRs as source of truth before coding. Text PR search missed Agno #8062 duplicates; issue-linked PRs revealed #8065/#8084 before the PR should have been opened.

## Execution Update: LiteLLM #28962

Checked after cloning `BerriAI/litellm` at `2026-05-27T09:20Z`.

- Upstream issue remained open with no comments/assignee.
- Open PR search for `28962 OR Gemini AI Studio 5xx MaskedHTTPStatusError ServiceUnavailableError` returned empty.
- Repo rules require CLA, one isolated scope, at least one mocked test in `tests/test_litellm/`, `make test-unit`, `make lint`, and Greptile review before maintainer review.
- Current `litellm_internal_staging` already maps a mocked Gemini AI Studio `MaskedHTTPStatusError` with status `503` to `ServiceUnavailableError` at both helper and public `acompletion` boundary.
- A `v1.85.1` worktree probe using the same public-boundary mock also mapped to `ServiceUnavailableError`.

Decision: no PR today. The next useful action is release/reporter follow-up only; if a real path still leaks, regression must reproduce that exact path, not only helper-level exception mapping.

## Execution Update: Playwright MCP #1635

Checked after cloning `microsoft/playwright-mcp` at `2026-05-27T09:35Z`.

- Upstream issue is open with a fresh CDP reproduction: `connectOverCDP` times out for default/`domcontentloaded`, but `waitUntil: "commit"` succeeds.
- Open PR search for `1635 OR browser_navigate_back waitUntil commit timeout` returned empty.
- `playwright-mcp` CONTRIBUTING says the MCP core moved to `microsoft/playwright` and unsolicited PRs without linked issue plus assignment/approval will be closed.
- Local wrapper checkout has only `src/README.md`; meaningful source/test work likely belongs in the Playwright monorepo under `packages/playwright/src/mcp` and `tests/mcp`.

Decision: create [corp-opensource#36](https://github.com/serejaris/corp-opensource/issues/36) as `COMMENT-FIRST / assignment-gated`. Upstream regression offer posted in [playwright-mcp#1635](https://github.com/microsoft/playwright-mcp/issues/1635#issuecomment-4552993898). Do not open a PR unless maintainers approve or assign.
