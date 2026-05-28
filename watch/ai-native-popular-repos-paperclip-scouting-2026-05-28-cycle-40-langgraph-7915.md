# Fresh AI-native issue scouting cycle 40, 2026-05-28

Scope: fresh issue scouting across popular AI-native framework/orchestration repos after the cycle 39 Pydantic watch. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills were not available in this environment, so this used the documented fallback: parent live GitHub gates, source read, 6 read-only subagent roles, then 3-role critique before any status decision.

Upstream actions: `0`. Runner actions: `0`.

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560897335).

Final `next_status`: `CANDIDATE` for `langchain-ai/langgraph#7915`, with explicit ownership-risk caveat. Not `COMMENT-FIRST` yet and not `PR-READY`.

## Parent Live Gates

Time window: `2026-05-28 04:40-04:52 UTC`.

| Repo / lane | Live state | Decision |
| --- | --- | --- |
| [`langchain-ai/langgraph#7915`](https://github.com/langchain-ai/langgraph/issues/7915) | Open, label `external`, no assignee. Issue reports `libs/sdk-py/langgraph_sdk/sse.py` drops required newlines between repeated SSE `data:` fields. The issue author already says they have a tested fix and asks maintainers for assignment. Targeted PR search for `#7915`, `SSEDecoder`, repeated `data`, and newline coverage found no exact open PR. Parent source read confirmed `SSEDecoder.decode()` appends each `data:` value via `_data.extend(value)` without inserting `\n`; existing tests cover `SSEDecoder` but not repeated `data:` fields. | `CANDIDATE-with-ownership-risk`. Strong source-valid bug and narrow regression surface, but no upstream PR/comment while the reporter's assignment request is active and runner evidence is missing. |
| [`run-llama/llama_index#21774`](https://github.com/run-llama/llama_index/issues/21774) | Open `bug`/`triage`, but already duplicate-covered by open PRs [`#21780`](https://github.com/run-llama/llama_index/pull/21780) and [`#21775`](https://github.com/run-llama/llama_index/pull/21775), both touching `initial_state` deepcopy paths. `#21775` has broad green checks except one Python 3.11 test failure; `#21780` is open/mergeable/review-required with no visible checks. | `WATCH / duplicate-covered`. |
| [`crewAIInc/crewAI#5886`](https://github.com/crewAIInc/crewAI/issues/5886) | Open bug about `cache_breakpoint` leaking to non-Anthropic providers. Covered/crowded by open PRs [`#5887`](https://github.com/crewAIInc/crewAI/pull/5887) and [`#5914`](https://github.com/crewAIInc/crewAI/pull/5914), both approved but currently conflicting, plus [`#5954`](https://github.com/crewAIInc/crewAI/pull/5954), open/mergeable with changes requested. | `WATCH / duplicate-covered`. No salvage/comment-first unless maintainer asks. |
| [`langchain-ai/langchain#37673`](https://github.com/langchain-ai/langchain/issues/37673) | Open `langchain-core` generator exhaustion bug. Multiple participants have fix-ready / assignment comments; prior PRs [`#37674`](https://github.com/langchain-ai/langchain/pull/37674) and [`#37680`](https://github.com/langchain-ai/langchain/pull/37680) closed without merge. | `WATCH / ownership-crowded`. |
| [`langchain-ai/langchain#37686`](https://github.com/langchain-ai/langchain/issues/37686) | Open `AIMessageChunk` non-ASCII serialization bug. Multiple assignment/fix-ready comments; PR [`#37691`](https://github.com/langchain-ai/langchain/pull/37691) closed without merge shortly after opening. | `WATCH / ownership-crowded`. |

## Six-role Synthesis

- Factology/source: `langgraph#7915` is the best uncovered lane. The source still concatenates repeated `data:` fields without the SSE-required newline separator, and the test surface is narrow.
- Duplicate/ownership: no exact PR was visible for `langgraph#7915`, but the reporter's tested-fix/assignment comment is an ownership signal; do not race it.
- Process: no `PR-READY` lane. A PR needs runner-backed fail-before evidence plus maintainer/assignment signal. Any future AI-assisted CrewAI PR would also need the repo's `llm-generated` process label.
- Actionability/repro: the minimal LangGraph regression is a `libs/sdk-py` `SSEDecoder` test with repeated `data:` lines that should decode to a newline-preserved payload.
- Docs/tracker: record a short watch note and umbrella tracker comment; no dedicated issue or README update yet.
- Challenger: keep `langgraph#7915` as `CANDIDATE`, not `COMMENT-FIRST`, because a comment now could look like reserving or taking over the reporter's lane.

## Three-role Critique

- Factology/duplicates: `langgraph#7915` can be internal `CANDIDATE`; exact cover PR search was clean, but ownership-risk must be explicit.
- Process gates: upstream PR is forbidden without runner evidence and maintainer go-ahead. Upstream comment is not a default action; only later if it adds maintainer-useful scope clarification rather than claiming work.
- Actionability: create this internal note and a short umbrella `#52` comment. Do not create a dedicated issue until runner evidence or maintainer signal makes the lane actionable.

## Re-entry Triggers

For `langgraph#7915`, re-enter only if one of these happens:

1. Maintainer explicitly asks for help or an alternate implementation.
2. The reporter does not open a PR after a reasonable assignment window and the issue remains open/unassigned.
3. An exact PR appears but closes without merge or lands without covering repeated `data:` newline semantics.
4. `corp-opensource-runner` becomes available and can produce a current-main fail-before regression for `libs/sdk-py`.

Minimal future test shape: feed `SSEDecoder` repeated `data:` lines and assert the decoded JSON payload preserves the `\n` separator, while retaining existing single-line `data:` behavior.
