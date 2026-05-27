# AI-native Frameworks Scouting Cycle 9

Checked at `2026-05-27T09:00Z`.

## Method

Six-subagent scouting pass plus local duplicate/rules checks where needed.

## Shortlist

| Rank | Target | Decision | Why | Next action |
|---:|---|---|---|---|
| 1 | [BerriAI/litellm#28962](https://github.com/BerriAI/litellm/issues/28962) Gemini AI Studio 5xx leaks as `MaskedHTTPStatusError` | `NO-GO / release-followup` | Fresh probes show current branch already maps mocked Gemini 503 `MaskedHTTPStatusError` to `ServiceUnavailableError` through `exception_type` and public `acompletion` | Watch reporter/upstream; do not open a PR unless the exact leaking path is confirmed |
| 2 | [modelcontextprotocol/python-sdk#2687](https://github.com/modelcontextprotocol/python-sdk/issues/2687) `AnyUrl` vs `AnyHttpUrl` OAuth redirect mismatch | `COMMENT-FIRST / maintainer-gated` | Pre-fix failure reproduced and patch surface is small, but repo rules require maintainer feedback / `ready for work` before PR | Wait maintainer confirmation/assignment |
| 3 | [microsoft/playwright-mcp#1635](https://github.com/microsoft/playwright-mcp/issues/1635) `browser_navigate_back` timeout after URL changed | `CANDIDATE / needs repro` | Excellent repo fit and deterministic browser/MCP surface; needs local repro and duplicate check before code | Add to future queue after current active PR load |
| 4 | [modelcontextprotocol/typescript-sdk#2155](https://github.com/modelcontextprotocol/typescript-sdk/issues/2155) `U+2028/U+2029` tool response timeout | `WATCH / duplicate-triage` | Strong wire/protocol bug signal, but open PRs [#1925](https://github.com/modelcontextprotocol/typescript-sdk/pull/1925) and [#1926](https://github.com/modelcontextprotocol/typescript-sdk/pull/1926) already cover server-side SSE escaping with tests and green CI | Watch maintainer review; only pursue uncovered client fail-fast / JSON-response gap |
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

## Execution Update: MCP TypeScript SDK #2155

Checked after cloning `modelcontextprotocol/typescript-sdk` at `2026-05-27T10:10Z`.

- Upstream issue is open, labeled `bug`, with no assignee and no linked PR in the issue timeline.
- Duplicate PR search found two exact server-side fixes:
  - [#1925](https://github.com/modelcontextprotocol/typescript-sdk/pull/1925) for `main` / v2: `packages/server/src/server/streamableHttp.ts`, regression test, changeset, mergeable, CI green, review required.
  - [#1926](https://github.com/modelcontextprotocol/typescript-sdk/pull/1926) for `v1.x`: `src/server/sse.ts`, `src/server/webStandardStreamableHttp.ts`, regression test, changeset, mergeable, CI green, review required.
- Repo rules say to comment before starting work to avoid duplicate effort; straightforward bugfixes can skip discussion only when not already contested.
- [Upstream triage comment](https://github.com/modelcontextprotocol/typescript-sdk/issues/2155#issuecomment-4553051842) posted linking #1925/#1926 and suggesting pkg-pr-new validation plus raw/escaped, stdio/Streamable HTTP, JSON-looking/plain text matrix.

Decision: create [corp-opensource#37](https://github.com/serejaris/corp-opensource/issues/37) as `WATCH / duplicate-triage`. Do not open a fresh PR for SSE escaping. A non-duplicate path needs proof of an uncovered client fail-fast or JSON-response-mode gap.

Regression-first lesson from this case: the useful test contract is not "Claude.AI should not hang", because that is outside the SDK and takes 300s to observe. The SDK-side regression is the deterministic wire contract: SSE transport must not emit literal U+2028/U+2029 inside a `data:` line; it should emit `\\u2028` / `\\u2029`, and `JSON.parse(data)` must still round-trip the original text.

## Execution Update: MCP Python SDK #2687

Checked after cloning `modelcontextprotocol/python-sdk` at `2026-05-27T09:15Z`.

- Upstream issue is open, with no labels, assignee, milestone, or linked fixing PR.
- Six-subagent triage converged: bug is real and current; no duplicate PR for this exact path.
- Adjacent PRs [#2630](https://github.com/modelcontextprotocol/python-sdk/pull/2630), [#2638](https://github.com/modelcontextprotocol/python-sdk/pull/2638), and [#1934](https://github.com/modelcontextprotocol/python-sdk/pull/1934) touch redirect URI validation but do not cover `AnyHttpUrl` stored vs incoming `AnyUrl` equality.
- Current `main` still has `OAuthClientMetadata.redirect_uris: list[AnyUrl] | None` and `validate_redirect_uri()` checks object membership.
- Direct no-secret repro with Pydantic 2.12.5 confirmed `str(AnyHttpUrl("https://example.com/cb")) == str(AnyUrl("https://example.com/cb"))`, but object equality is false and `validate_redirect_uri()` raises `InvalidRedirectUriError`.
- Repo rules require maintainer feedback or `ready for work` before starting; [upstream comment](https://github.com/modelcontextprotocol/python-sdk/issues/2687#issuecomment-4553121932) posted with proof, proposed model-boundary validator, and regression target.

Decision: create [corp-opensource#38](https://github.com/serejaris/corp-opensource/issues/38) as `COMMENT-FIRST / maintainer-gated`. Do not open PR until maintainers confirm or assign.

Regression-first card: test `tests/shared/test_auth.py::test_redirect_uri_subtype_is_validated_by_url_value`; pre-fix expected failure is `InvalidRedirectUriError`; post-fix expected pass is successful validation of incoming `AnyUrl` against registered `AnyHttpUrl` plus unchanged `model_dump(mode="json")`.
