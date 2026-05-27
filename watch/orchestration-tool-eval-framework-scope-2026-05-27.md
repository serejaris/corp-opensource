# Orchestration / tool / eval framework scope, 2026-05-27

Tracker: [#79](https://github.com/serejaris/corp-opensource/issues/79)

## Scope

Repo-scope scouting по agent orchestration, tool integration и eval/assertion framework surface:

- `crewAIInc/crewAI`
- `ComposioHQ/composio`
- `promptfoo/promptfoo`
- `microsoft/autogen`
- `stanfordnlp/dspy`

Обязательный skill `open-source-bug-scouting` в окружении не установлен, поэтому цикл выполнен documented fallback: 6 read-only subagents, parent live gates через GitHub API/`gh`, затем internal 3-role critique. Upstream comments/PR не создавались.

## Parent Live Gates

| Repo | Live signal, 2026-05-27 | Issue/PR pressure | Process / runner gate |
|---|---|---|---|
| `crewAIInc/crewAI` | MIT, Python, `52,312` stars, pushed `2026-05-27`, latest `1.14.5` on `2026-05-18` | API parent count `365`; PR count `326`, значит open issues около `39`. Fresh lanes `#5930/#5931/#5886` уже имеют nearby PRs `#5932/#5933/#5954`; pickle/security surface рядом с `#5946/#5950`. | `.github/CONTRIBUTING.md` требует `llm-generated` label для AI-authored PR/issue. Secret-free pytest possible, но full dependency matrix тяжелый; runner нужен для полного прогона. |
| `ComposioHQ/composio` | MIT по GitHub, TypeScript, default branch `next`, `28,484` stars, pushed `2026-05-27`, latest package release `@composio/claude-agent-sdk@0.9.2` on `2026-05-13` | API parent count `111`; PR count `83`, open issues около `28`. Fresh MCP/API/auth issues `#3485/#3482/#3477/#3471` выглядят hosted/backend dependent; `#3473` covers MCP empty-object schema. | Contribution text mentions ISC for contributions; branch/package-specific gate required. Meaningful e2e often needs `COMPOSIO_API_KEY`/provider secrets, so not runner-safe by default. |
| `promptfoo/promptfoo` | MIT, TypeScript, `21,661` stars, pushed `2026-05-27`, latest release `code-scan-action: 0.1.6` on `2026-05-21` | API parent count `321`; PR count `246`, open issues около `75`. Strong eval/tooling lanes, but fresh targets `#9467/#9458` covered by `#9479/#9484`; `#9235` now covered by `#9239/#9298`. | Best thematic fit, but high PR collision. Unit repro possible with `npm ci`/Vitest; full browser/share/smoke matrix needs runner. |
| `microsoft/autogen` | Code license note required: GitHub API reports `CC-BY-4.0`, repo also has `LICENSE-CODE` MIT. Python, `58,462` stars, pushed `2026-04-15`, latest `python-v0.7.5` on `2025-09-30` | API parent count `866`; PR count `338`, open issues около `528`. Shared memory `#7748` has draft `#7758`; docs/UTF-8 issue has PRs; current backlog noisy. | Microsoft CLA bot applies. Targeted Python package repro possible, but freshness/backlog make this watch-only. |
| `stanfordnlp/dspy` | MIT, Python, `34,688` stars, pushed `2026-05-27`, latest `3.2.1` on `2026-05-05` | API parent count `512`; PR count `233`, open issues около `279`. Tool/adapters/provider lanes are crowded: `#9686 -> #9814`, native tools `#9842`, `ParallelEvaluate #9574` covered by `#9807`. | CONTRIBUTING rejects fully autonomous AI-agent PRs and requires human-owned AI-assisted disclosure. Secret-free unit repro is good, LLM/Ollama paths need runner. |

## 6-subagent synthesis

1. Repo fit/activity: `promptfoo` ranked first for eval/red-team/agent trajectory surface; `dspy` second; `crewAI` third; `Composio` and `autogen` watch-only for this cycle.
2. Actionability: initial best lane was `promptfoo#9235` (`rag-poisoning` fails with `Unknown grader`), but it required maintainer intent before any PR.
3. Duplicate/process: `promptfoo`, `dspy` and `crewAI` are saturated with same-day PR overlap; `Composio` has fewer duplicates but more hosted-service blockers.
4. Community/process: hard gates are CrewAI `llm-generated` label, Composio branch/license wording, Microsoft CLA, DSPy human-owned AI disclosure, Promptfoo trust-boundary policy.
5. Runner/repro: `dspy` and `crewAI` have best secret-free unit repro; `promptfoo` is good for CLI/eval unit tests but full smoke/browser/share needs runner; Composio integration repro needs managed secrets.
6. Final synthesis before critique: `promptfoo` is the strongest future deep-dive, especially trajectory/assertion/redteam grader wiring, but needs exact duplicate gate before action.

## 3-role critique

- Фактология/дубликаты: `promptfoo#9235` нельзя считать open COMMENT-FIRST lane; it is directly covered by open PRs `promptfoo#9239` and `promptfoo#9298`, both referencing/fixing `#9235`.
- Process gates: upstream comment сейчас будет noise, потому что уже есть maintainer-facing discussion and reporter pressure around `#9298`; PR не разрешен без fresh repro and clean duplicate gate.
- Actionability: выбрать `WATCH / duplicate-race`; monitor `#9239/#9298` until merge/close/stall, then rerun gates if a gap remains.

## Decision

`next_status: WATCH`

Reason: repo fit is strong, but best issue-level promptfoo lead is duplicate-covered, CrewAI/DSPy have high overlap and policy gates, Composio is hosted-service heavy, and AutoGen is stale/high-backlog for quick contribution.

No runner repro was run because duplicate gate blocks before repro. `corp-opensource-runner` remains required for any later heavy promptfoo/browser/share smoke or DSPy/crewAI full matrix work; CT `216` was not used because this is not Hermes-specific.

## Follow-up

- Watch `promptfoo/promptfoo#9239` and `promptfoo/promptfoo#9298` for `#9235`.
- If both close/stall without fixing `Unknown grader: promptfoo:redteam:rag-poisoning`, rerun parent gates, run current-main secret-free repro in `corp-opensource-runner`, and only then reconsider `COMMENT-FIRST` or `CANDIDATE`.
- Keep `promptfoo` as preferred future eval/assertion/tooling deep-dive, but only after a clean issue-specific duplicate gate.
