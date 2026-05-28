# AI-native fresh discovery cycle 39: pydantic-ai #5696

Date: 2026-05-28 04:22 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560830327)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this cycle used the documented fallback: parent live GitHub gates, 6 read-only subagents, then a 3-role critique.

## Scope

Bounded fresh search across high-value AI-native repos after the 04:17 active PR dashboard refresh. The search focused on open bug issues updated since 2026-05-27 in:

- `pydantic/pydantic-ai`, `openai/openai-agents-python`, `OpenHands/OpenHands`, `cline/cline`, `anomalyco/opencode`
- `google-gemini/gemini-cli`, `vercel/ai`, `modelcontextprotocol/inspector`, `PrefectHQ/fastmcp`, `ChromeDevTools/chrome-devtools-mcp`, `browser-use/browser-use`
- `browseros-ai/BrowserOS`, `simular-ai/Agent-S`, `web-infra-dev/midscene`, `microsoft/playwright-mcp`, `browserbase/stagehand`, `trycua/cua`

## Live gates

Selected watch lane: [pydantic/pydantic-ai#5696](https://github.com/pydantic/pydantic-ai/issues/5696), `[roundtrip-sweep] ModelRequest.parts: InstructionPart fails to deserialize after serialization`.

- Issue state: open.
- Created: 2026-05-28 04:18 UTC by `app/github-actions`.
- Labels: `bug`, `durable exec`, `pydanty:bug`, `serialization`.
- Assignee: none.
- Latest visible issue comment: `pydanty` classified the issue and said the next step is delegating to the sweep reproducer.
- Repo gate: `pydantic/pydantic-ai` is MIT, default branch `main`, about 17.3k stars, `CONTRIBUTING.md` exists.
- Source gate: current `pydantic_ai_slim/pydantic_ai/messages.py` has `InstructionPart.part_kind = 'instruction'`; `_model_request_part_discriminator()` falls through to `part_kind`; `ModelRequestPart` includes `SystemPromptPart`, `UserPromptPart`, `ToolSearchReturnPart`, `ToolReturnPart`, and `RetryPromptPart`, but not `InstructionPart`.
- Duplicate PR gate: searches for `InstructionPart`, `ModelRequestPart`, `roundtrip-sweep`, `ModelMessagesTypeAdapter`, and `instruction deserialize serialization` found no exact open PR.
- Related issue gate: [#5282](https://github.com/pydantic/pydantic-ai/issues/5282) also mentions instructions, but is about `TemporalDynamicToolset` instruction loss, not `ModelMessagesTypeAdapter` JSON deserialization.

Runner gate: no repro or tests were run. `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10); this cycle stopped at read-only watch because pydanty automation already owns the immediate upstream next step.

## Candidate map

| Lane | Live result | Decision |
|---|---|---|
| `pydantic-ai#5696` | Fresh open serialization/durable-exec bug with MRE and small likely fix surface; no exact PR found. Automation/pydanty has already started a reproducer/sweep flow. | `WATCH / automation-race`, not `CANDIDATE` and not `PR-READY`. Re-enter after sweep outcome or stale-window plus runner repro. |
| `openai-agents-python#3512` | Exact-cover PR [#3513](https://github.com/openai/openai-agents-python/pull/3513) is open and says `Fixes #3512`. | `WATCH / duplicate-covered`; no upstream action. |
| `pydantic-ai#5688` | Already tracked; exact-cover PR [#5694](https://github.com/pydantic/pydantic-ai/pull/5694) is live from prior cycles. | `WATCH / duplicate-covered`; no duplicate PR. |
| `browser-use#4696` | Open video-speed issue, but weak technical shape and lower Paperclip-like value. | `NO-GO for this cycle`; revisit only with sharper browser/action contract. |
| `trycua#1725` | Already tracked Windows click marker lane. | Existing `WATCH`; no fresh target. |

## 6-role synthesis

- Factology: `#5696` is a plausible real bug. The current source-level contract supports the report: the discriminator can return `instruction`, but the request-part union does not register that tag.
- Duplicate/race: no exact PR is visible now, but race risk is high because the issue is automation-authored and pydanty already delegated reproducer/sweep work.
- Process: upstream PR/comment is not justified. Upstream already has an MRE, classification and next step; a comment would add noise unless we later have independent current-main evidence after the automation window.
- Actionability: a focused unit test would likely be small: `ModelMessagesTypeAdapter.dump_json()` and `validate_json()` around `ModelRequest(parts=[..., InstructionPart(...), ...])`. Do not mix in the secondary `BinaryImage -> BinaryContent` demotion without maintainer direction.
- Rejected lanes: `openai-agents#3512` and `pydantic#5688` are exact-covered; `browser-use#4696` is low-fit; `trycua#1725` is already tracked.

## 3-role critique

- Factology/duplicates: source-level bug claim is strong and no exact open PR was found, but the pydanty automation handoff makes ownership unsafe.
- Process gates: internal watch note, README/repo-card update and one umbrella `#52` comment are allowed. No upstream comment, PR, ping or repro claim.
- Actionability: final status must stay `WATCH`, not `CANDIDATE`, because no runner-backed current-main repro was run and the upstream automation window has not cleared.

## Decision

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Re-entry triggers:

1. `pydantic-ai#5696` gets a linked PR/comment from pydanty or maintainers, then recheck exact cover.
2. No PR appears after a short stale-window, then run current-main repro in `corp-opensource-runner` and repeat duplicate/contribution gates.
3. Maintainer explicitly asks for an external patch or test.
