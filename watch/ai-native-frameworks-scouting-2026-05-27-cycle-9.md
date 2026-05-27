# AI-native Frameworks Scouting Cycle 9

Checked at `2026-05-27T09:00Z`.

## Method

Six-subagent scouting pass plus local duplicate/rules checks where needed.

## Shortlist

| Rank | Target | Decision | Why | Next action |
|---:|---|---|---|---|
| 1 | [BerriAI/litellm#28962](https://github.com/BerriAI/litellm/issues/28962) Gemini AI Studio 5xx leaks as `MaskedHTTPStatusError` | `GO / regression-first` | Small provider error-mapping bug, no open PR found by PR-readiness lane, mocked unit test likely enough | Create internal issue, clone, write failing mocked regression before patch |
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
