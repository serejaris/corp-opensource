# AI-native popular repo universe, cycle 25, 2026-05-27

Live state: parent `gh` pass на 2026-05-27 17:57 UTC.
Tracker: [serejaris/corp-opensource#52](https://github.com/serejaris/corp-opensource/issues/52).

Upstream actions в этом блоке: 0 PR, 0 comments.

Final `next_status`: `WATCH`.

Cycle 25 является bounded repo/issue scouting по ранее promoted lane `openai/openai-agents-python`. Репозиторий остаётся сильным AI-native/Paperclip-adjacent target для continued scouting, но этот цикл не нашёл issue-level `PR-READY` target. Из-за активной duplicate-race по самым узким bugs и отсутствия fresh current-main repro по clean realtime leads upstream comment/PR запрещены.

`open-source-bug-scouting` и `open-source-pr-workflow` skills в текущей среде не установлены. Documented fallback: 6 subagent scouting roles через доступный `multi_agent_v1`, parent live gates через `gh`, затем 3 subagent critique. Runner/repro: N/A для repo-level scan; для issue-level realtime leads нужен отдельный current-main repro cycle, желательно в dedicated runner, но конкретный reproducible target в этом блоке не выбран.

## Cycle 1: Six-subagent scouting

Roles:

- repo-fit;
- bug-signal;
- repro-path;
- patchability;
- duplicate-race;
- PR-readiness.

Synthesis:

- `openai/openai-agents-python` хорошо подходит под AI-native strategy: Agents SDK, tools, handoffs, tracing, sessions, realtime, sandboxes, MCP, structured output and hook/runtime surfaces.
- Repo activity strong: latest live parent snapshot показал `26705` stars, pushed `2026-05-26T08:53:37Z`, release `v0.17.4`, `45` open issues, `68` open PRs.
- Test surface пригоден для no-secret unit regressions: `FakeModel`, fake response builders, fake MCP server/helpers, mock OpenAI clients and `httpx.MockTransport` patterns.
- Contribution surface visible: `AGENTS.md`, PR template, issue templates, Makefile/CI commands, Python `3.10`-`3.14` matrix, Windows tests and docs build.
- High duplicate velocity: fresh narrow bug reports quickly acquire PRs, so issue-level duplicate gate must be repeated immediately before comment/code.

## Parent Live Gates

Authoritative snapshot uses `gh repo view`, `gh issue view`, `gh pr list`, and targeted `gh pr view` checks. `gh pr view` in this environment does not expose `closingIssuesReferences`, so closing relationships are not asserted from that field here.

| Target | Live state | Decision |
|---|---|---|
| Repo `openai/openai-agents-python` | `26705` stars, pushed `2026-05-26`, release `v0.17.4`, `45` open issues, `68` open PRs. | Strong repo-fit, but not issue-level ready. |
| `#3512` / `#3513` | `#3512` is fresh `bug`; active PR `#3513` is open, non-draft, mergeable in tracked state. | `WATCH` / active competing PR; no duplicate PR/comment. |
| `#3348` / `#3349` / `#3498` / `#3508` | AdvancedSQLiteSession false-success area has several open PRs; `#3508` is open, non-draft, mergeable in tracked state. | `NO-GO for new PR`; duplicate-race. |
| `#3356` / `#3373` | Realtime known tool failure output issue has open draft PR `#3373`; PR is conflicting in tracked state. | `WATCH`; prior art/author lane exists. |
| `#3477` / `#3486` / `#3480` | MCP/custom metadata issue has maintainer path through `#3486` and draft/alternative `#3480`. | `WATCH`; no new PR. |
| `#1845` | Open, `feature:realtime`, 1 comment, no assignee; last updated `2025-10-02`. | `LEAD/WATCH`; needs fresh current-main repro and trace overlap check, including `#2477`. |
| `#1168` | Open, `bug` + `feature:realtime`, 1 comment, no assignee; last updated `2025-07-25`. | `LEAD/WATCH`; needs fresh current-main realtime repro and duplicate search against later realtime PRs. |

## Candidate Ranking

| Candidate | Status | Why |
|---|---|---|
| `#3512` RunHooksBase `on_tool_end` structured result | `WATCH` | Excellent unit-test candidate, but active competing PR `#3513` already exists. |
| `#3348` AdvancedSQLiteSession metadata failure false-success | `NO-GO for new PR` | Multiple active PRs in same area; new PR would be duplicate-race. |
| `#3356` realtime known tool failure output | `WATCH` | Relevant runtime reliability bug, but draft conflicting PR `#3373` exists. |
| `#3477` MCP tool response metadata | `WATCH` | Maintainer-driven path already exists; not a fresh bugfix lane. |
| `#1845` RealtimeAgent empty trace spans | `LEAD` | No direct competing PR found in this pass, but old and needs fresh no-secret/current-main repro plus overlap check. |
| `#1168` realtime multiple tool calls no voice output | `LEAD` | Direct duplicate not found in this pass, but old, realtime/API-dependent, and may overlap with later realtime tool-output PRs. |

## 3-subagent critique

- factology/duplicates: repo facts are a timestamped snapshot only. Do not write that `#3512` or `#3348` are "fixed"; use active competing PR / duplicate-race wording until merge. Do not promote `#1845` or `#1168` above `LEAD/WATCH` without fresh repro on `v0.17.4` / current `main`.
- process gates: before writing this artifact, cycle 25 was incomplete because no local synthesis file existed. After this watch note and tracker update, the cycle is valid only as repo-level scouting with 0 upstream actions.
- actionability: choose exactly `WATCH`. Unblock event: one concrete `openai/openai-agents-python` bug with fresh current-main repro, clean duplicate/competing-PR gate, and no need to expose real API secrets in repro.

## Next Bounded Cycle

Take exactly one realtime lead, preferably `#1845` or `#1168`, and run a narrow current-main repro/duplicate gate:

1. Check issue still open, labels, assignee, timeline, linked PRs, and PR search.
2. Compare against `v0.17.4` and current `main`.
3. Decide whether a secret-safe unit/harness repro exists; if not, write `NO-GO / needs-live-realtime-evidence`.
4. Only if repro is fresh and duplicate gate clean, create a regression card with test file, command, pre-fix fail, post-fix pass. Until then, no upstream comment or PR.
