# AI SDK / Agents repo scope check, 2026-05-27

Tracker: [#66](https://github.com/serejaris/corp-opensource/issues/66), umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52)

Итоговый `next_status`: `WATCH`.

Этот bounded cycle проверил `vercel/ai` и `cloudflare/agents` как популярные AI-native repo для Paperclip-like strategy. Оба repo релевантны, но ни один не повышается до `CANDIDATE`: нет one-issue duplicate gate, runner repro и regression card.

## Parent live gates

### `vercel/ai`

- Repo: created `2023-05-23`, default branch `main`, not archived/fork.
- Activity: `24500` stars, `4483` forks, `976` issues, `668` PRs; pushed `2026-05-27 20:29 UTC`.
- Release signal: latest release `@ai-sdk/mcp@1.0.44`, published `2026-05-27 17:32 UTC`.
- License/process: GitHub reports `Other/NOASSERTION`, but repo `LICENSE` is Apache License 2.0. Community profile `87%`; README, CONTRIBUTING, CODE_OF_CONDUCT and PR template present.
- Contribution/test gate: Node `22/24/26`, `pnpm@10`, signed commits, patch changeset for package changes, package-level tests plus relevant root checks.
- Runner gate: current local env is not enough (`node v20.19.2`, `npm 9.2.0`, no `pnpm`). Use runner for dependency-heavy repro.

Watch leads, not candidates:

- `#15652`: Anthropic `disableParallelToolUse` hardcoded with `structuredOutputMode: jsonTool`.
- `#15621`: `audio/mp4` signature offset validation.
- `#15613`: Vertex conversion drops provider-executed `code_execution` tool result.
- `#15430`: `streamText` / ToolLoopAgent stream hang on cancelled fetch body.
- Secondary exact lanes: `#15248`, `#15155`, `#14932` only after separate duplicate gate.

Duplicate/noise blockers:

- Anthropic base URL is covered by open PRs `#15545`, `#15581`, `#15586`.
- Gemini embedding GA is covered by `#15584/#15587`.
- Readonly array type issue is covered by `#15594`; Gradium provider by `#15598`.
- MCP/error/auth/spawn and streaming lanes have nearby open PRs such as `#15616`, `#15591`, `#15568`, `#15495`, `#15447`, `#15441`, `#15434`.
- Provider-add, docs/support and promotional issues are not opportunistic PR targets.

Decision: `WATCH`, exact fixture/unit-safe provider/schema/tool-call/streaming validation only. No generic PR hunting.

### `cloudflare/agents`

- Repo: created `2025-01-29`, default branch `main`, not archived/fork, MIT.
- Activity: `4996` stars, `563` forks, `93` issues, `18` PRs; pushed `2026-05-27 19:43 UTC`.
- Release signal: latest release `@cloudflare/voice@0.2.1`, published `2026-05-26 17:54 UTC`.
- Fit: direct agent runtime with Durable Objects, subagents, MCP, streaming, workflows, compaction, codemode and sandbox surfaces.
- Community/process: community profile `87%`; README and MIT license present. README says external pull requests are not accepted currently because the SDK is evolving quickly; preferred action is bug reports / feature requests / discussions.
- Runner gate: current local env is not enough (`node v20.19.2`, `npm 9.2.0`, no `wrangler`). Proper repro needs Node `24+`, npm/Nx, Workers/Vitest pool, possibly Playwright and Wrangler-compatible runtime.

Watch leads, not candidates:

- `#1595`: `onStart` recovery cascade exceeds `blockConcurrencyWhile` budget and wedges parent Durable Object.
- `#1593`: auto-compaction trigger fires, but compaction does not run due internal token counting.
- `#1591`: websocket client buffers text between tool calls instead of streaming.
- `#1586`: `addToolOutput` / client-tool auto-continuations attach only as end-of-turn replay.
- `#1583`: MCP SSE keepalive missing/broken on Cloudflare edge.
- `#1539`: `Workspace.rm` recursive query can be a narrower implementation/perf lead.

Duplicate/process blockers:

- `#1564` stable MCP server ids is covered by PR `#1596`.
- MCP/codemode transport and migration areas have active maintainer/draft PRs such as `#1556`, `#1557`, `#1581`, `#1566`.
- Tool-output/continuation, websocket and sub-agent areas have substantial nearby merged or draft work.
- Security-adjacent issues, deployed Workers behavior, Cloudflare account/API-token paths, auth, voice, x402 and provider-live paths need a separate security/runner/secrets gate.

Decision: `WATCH / issue-first`. Treat as runtime reference and exact bug watch, not an upstream PR target unless maintainers explicitly invite/allow a narrow fix.

## 6-subagent synthesis

1. Repo fit/activity: both repos are active and AI-native. `vercel/ai` has stronger popularity; `cloudflare/agents` has stronger Paperclip-like runtime fit.
2. Issue quality: both have credible watch leads, but none are candidates without one-issue duplicate gate and repro.
3. PR/duplicate risk: `vercel/ai` is duplicate-heavy across provider/MCP/streaming lanes; `cloudflare/agents` has smaller but maintainer-driven PR flow.
4. Process/license/community: `vercel/ai` passes Apache-2.0-with-GitHub-NOASSERTION note; `cloudflare/agents` passes MIT but external PR policy blocks generic PR hunting.
5. Runner/repro: local environment is insufficient for both. Use dedicated runner and secret-safe fixtures before any promotion.
6. Actionability: final status `WATCH`; update repo-scope/cards and do not comment upstream.

## 3-subagent critique

- Factology: do not call any listed issue a candidate. Use "watch lead" until duplicate gate, repro and regression card exist.
- Duplicates: explicitly note already covered `vercel/ai` lanes and maintainer/draft work in `cloudflare/agents`.
- Process: `cloudflare/agents` external PR policy means action mode is issue-first / maintainer-signal only.
- Runner: record Node/pnpm/Wrangler gaps; no tests or repro were run in this cycle.

## Vercel AI heartbeat - 2026-05-28 01:45 UTC

The repo-universe fallback cycle rechecked two exact `vercel/ai` provider lanes:

- `#15652`: open/no comments; targeted PR search found no exact cover for Anthropic `structuredOutputMode: "jsonTool"` ignoring `disableParallelToolUse: false`. Critique promoted this single lane to internal `CANDIDATE`, blocked on Node/pnpm current-main fixture and changeset/test gate.
- `#15613`: open/no comments; no exact PR found for Vertex `code_execution` provider-executed tool-result round-trip, but the lane is broader and provider-fixture-heavy. Critique kept it `WATCH` until repro.

Upstream action count remains `0`; no cold PR/comment before duplicate gate, runner-backed regression card and package-level tests.

## Vercel AI readiness heartbeat - 2026-05-28 01:08 UTC

`vercel/ai#15652` remains open, unassigned, and uncommented. No exact PR was found for Anthropic `structuredOutputMode: "jsonTool"` forcing `disableParallelToolUse: true`. Source-path confidence is high in `packages/anthropic/src/anthropic-language-model.ts`, but adjacent provider/structured-output PRs `#10274/#15112/#10812/#12903` remain watch risks.

Cycle 38 follow-up `2026-05-28 04:07 UTC`: [watch note](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-38-fresh-discovery.md), selected tracker [#86](https://github.com/serejaris/corp-opensource/issues/86), umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560779972). Six read-only roles plus 3-role critique selected `vercel/ai#15652` as the internal package-fixture `CANDIDATE`; upstream action count remained `0`. Caveat: a fixture can prove request serialization ignores the user override, but it must not overclaim that maintainers must accept parallel real-tool fan-out on the synthetic `jsonTool` path.

Decision remains `next_status: CANDIDATE`; upstream action count `0`. Promotion requires a Node/pnpm current-main package fixture, patch changeset/signed-commit readiness, and fresh adjacent PR search.

## Final decision

- `vercel/ai`: `WATCH`, high-value exact fixture-validation watch; no generic provider/docs/support PR hunting.
- `cloudflare/agents`: `WATCH`, issue-first runtime/MCP/compaction reference watch; no PR without maintainer signal.
- Upstream action: none.
