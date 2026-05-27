# AI-native Frameworks Scouting Cycle 8

Checked at `2026-05-27T08:05Z`.

## Method

Six-subagent scouting pass plus local verification. Subagent lanes:

- repo-fit/trending repositories;
- Python agent frameworks;
- browser/runtime/container boundaries;
- coding-agent CLI/runtime guards;
- Paperclip-like harness/control-plane opportunities;
- stale tracker audit.

## New Decisions

| Target | Upstream | Internal | Decision | Why |
|---|---|---|---|---|
| Agno MCP stream default crash | [agno#8062](https://github.com/agno-agi/agno/issues/8062) | [#30](https://github.com/serejaris/corp-opensource/issues/30) | `GO / needs local repro` | Large active repo, no duplicate PR found, narrow MCP tool boundary, likely testable without live LLM |
| LangGraph Postgres numeric filters | [langgraph#7684](https://github.com/langchain-ai/langgraph/issues/7684) | [#31](https://github.com/serejaris/corp-opensource/issues/31) | `NO-GO / watch only` | Three existing PR attempts (#7685, #7722, #7909) were auto-closed by assignment gate |
| CUA driver MCP SIGABRT | [trycua#1724](https://github.com/trycua/cua/issues/1724) | [#32](https://github.com/serejaris/corp-opensource/issues/32) | `COMMENT-FIRST / needs design confirmation` | No duplicate PR, but first need upstream confirmation that direct stdio launch is supported |
| Codex search history provider switch | [codex#24612](https://github.com/openai/codex/issues/24612) | [#33](https://github.com/serejaris/corp-opensource/issues/33) | `WATCH / access-sensitive candidate` | No direct PR found; provider-history sanitizer regression is clear, but upstream contribution path is constrained |
| Goose stdio MCP subprocess SIGTERM | [goose#9332](https://github.com/aaif-goose/goose/issues/9332) | [#34](https://github.com/serejaris/corp-opensource/issues/34) | `COMMENT-FIRST / needs Linux repro` | No direct PR found; design-sensitive process cleanup around `PR_SET_PDEATHSIG` and multi-threaded Tokio |

## Other Signals

- `pydantic-ai#5671` remains a strong future candidate, but we already have two active pydantic PRs waiting review. Do not open another until the active pydantic queue changes.
- `browser-use#4846` is covered by active PRs #4847/#4881; review/validation only.
- `OpenHands#14563` remains maintainer-direction-needed; likely Docker/container visibility but overlaps personal-skills roadmap.
- `E2B#1352` remains strategically relevant for sandbox reconnect, but needs live SDK/server repro before PR.
- `pydantic-ai#5671` and `Codex#24612` are both provider-history/request-shaping bugs; future tests should assert final provider payload, not helper-only predicates.

## Assignment Gate Lesson

LangGraph #7684 is a clean bug but a bad fresh-PR target today. In repos that auto-close external PRs unless the author is assigned:

- do not open a new PR if several authors already posted equivalent assignment-gated PRs;
- record the duplicate PRs and wait for maintainer assignment/reopen;
- if contributing later, start by asking on the issue with a specific missing test or consolidation offer.

## Next Best Action

Update after execution (`2026-05-27T08:27Z`): [corp-opensource#30](https://github.com/serejaris/corp-opensource/issues/30) produced a valid local regression and patch, but upstream [agno#8124](https://github.com/agno-agi/agno/pull/8124) was auto-closed as duplicate because [agno#8065](https://github.com/agno-agi/agno/pull/8065) and [agno#8084](https://github.com/agno-agi/agno/pull/8084) already referenced issue #8062. The useful delta is our integration-style MCP tool registration test; it was offered upstream in [this issue comment](https://github.com/agno-agi/agno/issues/8062#issuecomment-4552788085).

Process correction: text `gh search prs` is not enough for issue-based duplicate gates. Check linked/cross-referenced PRs on the target issue before coding and again before `gh pr create`.

Next active candidates after #30:

1. [corp-opensource#34](https://github.com/serejaris/corp-opensource/issues/34) goose `PR_SET_PDEATHSIG` / MCP subprocess lifecycle: Linux repro + maintainer approach first.
2. [corp-opensource#32](https://github.com/serejaris/corp-opensource/issues/32) trycua `cua-driver mcp` SIGABRT: comment-first unless direct stdio launch support is confirmed.
3. [corp-opensource#33](https://github.com/serejaris/corp-opensource/issues/33) Codex provider-history crash: watch/access-sensitive; avoid PR unless contribution path is clear.
