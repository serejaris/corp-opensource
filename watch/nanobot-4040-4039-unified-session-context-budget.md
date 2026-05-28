# Nanobot #4040/#4039 unified session and context budget

Дата: 2026-05-28

Upstream:

- https://github.com/HKUDS/nanobot/issues/4040
- https://github.com/HKUDS/nanobot/issues/4039

Trackers:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561501637

Статус: `CANDIDATE`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Fresh discovery after `2026-05-28T06:41:00Z` found two new `HKUDS/nanobot` issues with stronger fit than the previous duplicate-covered Nanobot lanes:

- `#4040`: `/stop` may fail to cancel the active task when `unified_session = true`, because cancellation appears to use the raw message session key instead of the effective unified session key.
- `#4039`: context snipping appears to include tool definitions in the initial estimate but fails to reserve tool-schema tokens when computing the retained-history budget, so the final prompt can remain over the model context window.

`#4040` is the stronger candidate because it is a runtime interrupt/cancellation bug with a narrow test shape. `#4039` is strategically relevant to MCP/tool-schema-heavy agents, but needs code-path confirmation around token estimation and trimming.

## Parent live gates

- Repo: `HKUDS/nanobot`, ~43k stars, active `main`, top-level `CONTRIBUTING.md`, `LICENSE`, `README.md`, `SECURITY.md`, `pyproject.toml`; GitHub API license detection absent.
- `#4040`: open, unassigned, no labels, no comments, created `2026-05-28T06:43:13Z`.
- `#4039`: open, unassigned, no labels, no comments, created `2026-05-28T06:43:12Z`.
- Duplicate/PR search: no exact open PR or duplicate issue found for `/stop` + `unified_session` cancellation or tool-schema token reservation in retained-history budget.
- Prior repo state: cycle 34 kept `HKUDS/nanobot` in `WATCH` because `#4006/#4013` were exact-covered by open PRs `#4011/#4020`. These two fresh issues are distinct and not covered by those PRs.
- Runner: `corp-opensource-runner` remains unavailable via tracker `#10`; no repro/test was run.

## Duplicate and prior-art notes

`#4040`:

- No exact issue/PR cover found by `/stop`, `unified_session`, cancellation, raw/effective session-key searches.

`#4039`:

- Adjacent prior art exists around context governance/token estimation, including older hard context-budget and split context-governance work, but no exact cover for "subtract tool-schema tokens from retained-history budget" was found.
- This makes `#4039` a valid candidate, but lower confidence than `#4040` until source inspection confirms the estimation path.

Other fresh leads from this window:

- `Yeachan-Heo/oh-my-claudecode#3163`: valid tool-hook/token-noise lead, but quality-of-life and lower strategic weight.
- `github/gh-aw#35423`: generated low-severity CLI/DX issue; useful as agentic workflow tooling context, not a runner-first candidate.

## Repro plan

For `#4040`:

1. Enable `unified_session`.
2. Register or start a long-running fake task under the effective unified session key.
3. Issue `/stop` from a raw channel session key.
4. Assert cancellation resolves through the same effective key and removes/cancels the active task.
5. Add a regression test around the priority command path before/after effective-key normalization.

For `#4039`:

1. Build a small context window fixture with large fake tool schemas, system prompt tokens, and message history.
2. Trigger context snipping.
3. Assert the retained-history budget subtracts all non-history prompt components: system messages, tool definitions/MCP schemas, and provider overhead if modeled.
4. Re-estimate the full prompt after trimming and assert it fits the effective input budget.

## 6-role synthesis

Fresh discovery found four material issues after `06:41 UTC`, but only the Nanobot pair clears the bar for a candidate update. `#4040` is a direct agent-control failure; `#4039` is a context-budget/tool-schema failure. Both are open, unassigned, uncovered, and distinct from prior duplicate-covered Nanobot issues.

Existing runner backlog remains Probe/Gemini/Vercel/LangChain4j first, because those lanes are already tracked and runner-shaped. Nanobot `#4040` is a strong fallback candidate if source inspection confirms the effective-key path. Nanobot `#4039` is a reserve candidate pending source confirmation.

## Critique

- Factology/duplicates: pass. No exact cover found. Prior Nanobot duplicate-covered lanes do not cover these fresh issues.
- Process gates: no upstream comment/PR. Issue bodies already include root-cause hypotheses and suggested fixes; without new repro/log/test evidence, an upstream comment would add noise.
- Actionability: `#4040` is high-actionability candidate; `#4039` is weaker but still candidate-level as a paired context/tool-schema budget lane.

## Decision

`next_status: CANDIDATE`

Do not open upstream PR or comment. First useful action is runner/source-backed repro, with `#4040` ahead of `#4039`.
