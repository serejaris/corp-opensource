# AG-UI scouting, 2026-05-27

## Snapshot

- Время live gates: 2026-05-27 18:50-19:10 UTC.
- Upstream: [`ag-ui-protocol/ag-ui`](https://github.com/ag-ui-protocol/ag-ui).
- Internal tracker: [#56](https://github.com/serejaris/corp-opensource/issues/56), umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52).
- Итог цикла: `next_status: CANDIDATE`.
- Upstream PR/comment: 0.
- Runner repro/test: не запускался. Причина: цикл был issue-level scout, не `PR-READY`; следующий обязательный gate для `#1635` - secret-free unit repro in `corp-opensource-runner` or documented equivalent before any upstream action.
- Required `open-source-bug-scouting` skill недоступен в этой сессии; fallback: 6-subagent scouting + parent `gh` gates + 3-subagent critique.

## Parent Live Gates

- Repo активный, не archived: 13,878 stars, 1,242 forks.
- Default branch: `main`; license MIT.
- Latest release at gate: `release/2026-05-26`, published 2026-05-26 03:42 UTC.
- Open queue at gate: 173 issues, 130 PRs.
- Fresh checkout for source/process read: `/tmp/ag-ui-scout` at `f5bfba8` (`Merge pull request #1798 from ag-ui-protocol/feat/langgraph-inject-forwarded-headers`), 2026-05-27 18:50 UTC.
- Package manager/workspace: `pnpm@10.33.4`, Node `>=18`, Nx monorepo.
- Relevant package: `integrations/mastra/typescript/package.json`, package `@ag-ui/mastra` version `1.0.3`, test command `pnpm --filter @ag-ui/mastra test`.

## Contribution Gate

AG-UI `CONTRIBUTING.md` is issue-first / assignment-first:

- file an issue first for fixes/features/docs;
- ask to be assigned and tag/coordinate with code owners;
- significant work should be aligned with maintainers before PR;
- PR should include `Fixes #<issue-number>`;
- external e2e/preview checks may require maintainer authorization or internal follow-up PR.

Therefore `#1635` can be internal `CANDIDATE`, but not `PR-READY` until runner evidence, fresh duplicate gate, and maintainer assignment/approval are recorded.

## Candidate Lane

| Lane | Status | Gate |
|---|---|---|
| [`#1635`](https://github.com/ag-ui-protocol/ag-ui/issues/1635) `@ag-ui/mastra` aborts SSE on payload-less Mastra chunk | internal `CANDIDATE` | Open, no labels, no assignee, no comments, unchanged since 2026-05-05. Current `main` still has fatal `!chunk || !chunk.payload` guard in `integrations/mastra/typescript/src/mastra.ts`; payload-less `data-workspace-metadata` aborts before switch/default. No exact PR duplicate found by parent search. Blocked on runner repro, fresh duplicate gate, and assignment/approval before PR. |

## Watch / No-Go Lanes

| Lane | Status | Gate |
|---|---|---|
| [`#1776`](https://github.com/ag-ui-protocol/ag-ui/issues/1776) LangGraph sequential parallel tool-call args | `NO-GO / duplicate-merged` | Covered by merged [`#1760`](https://github.com/ag-ui-protocol/ag-ui/pull/1760); [`#1777`](https://github.com/ag-ui-protocol/ag-ui/pull/1777) closed as duplicate. Issue remains open but lane is not free. |
| [`#1584`](https://github.com/ag-ui-protocol/ag-ui/issues/1584) duplicate `RUN_STARTED` on interrupt replay | `WATCH / likely fixed-on-main` | No direct PR found, but current `main` dispatch order and `test_prepare_stream_interrupt_resume.py` appear to cover one `RUN_STARTED`; stale/fixed-on-main check only. |
| [`#1720`](https://github.com/ag-ui-protocol/ag-ui/issues/1720) client `flushState` multi-batch state | `WATCH / duplicate-covered` | Covered by open [`#1721`](https://github.com/ag-ui-protocol/ag-ui/pull/1721). |
| [`#1748`](https://github.com/ag-ui-protocol/ag-ui/issues/1748), [`#1749`](https://github.com/ag-ui-protocol/ag-ui/issues/1749), [`#1742`](https://github.com/ag-ui-protocol/ag-ui/issues/1742) LangGraph prepare/tool message fixes | `WATCH / duplicate-covered` | Covered by open PRs [`#1751`](https://github.com/ag-ui-protocol/ag-ui/pull/1751), [`#1752`](https://github.com/ag-ui-protocol/ag-ui/pull/1752), [`#1764`](https://github.com/ag-ui-protocol/ag-ui/pull/1764). |
| [`#1668`](https://github.com/ag-ui-protocol/ag-ui/issues/1668) Spring AI deferred tool events | `WATCH / duplicate-covered` | Covered by open [`#1734`](https://github.com/ag-ui-protocol/ag-ui/pull/1734). |
| [`#1570`](https://github.com/ag-ui-protocol/ag-ui/issues/1570) machine-readable JSON schemas | `COMMENT-FIRST-only` | Useful protocol/schema direction, but broad design lane. Needs canonical source/versioned URL/generation pipeline decision before code. |
| [`#1568`](https://github.com/ag-ui-protocol/ag-ui/issues/1568) WebSockets docs | `COMMENT-FIRST-only` | Documentation lane but scope ambiguous: concept docs vs minimal example vs runtime adapter. |
| [`#1644`](https://github.com/ag-ui-protocol/ag-ui/issues/1644) clone/perf subscriber issue | `WATCH / stale-check` | Current `main` appears to have changed subscriber cloning behavior; needs published-version/current-main comparison before any claim. |

## Regression Card For `#1635`

- Contract: a payload-less custom Mastra chunk such as `{ type: "data-workspace-metadata", data: {...} }` must not call `onError`, must not return `true` from chunk handling, and must not prevent later `text-delta` chunks from becoming AG-UI text events.
- Current code evidence: `integrations/mastra/typescript/src/mastra.ts` lines around 556-564 call `callbacks.onError(new Error("Malformed stream chunk... missing payload"))` when `!chunk || !chunk.payload`.
- Existing test anchor: `integrations/mastra/typescript/src/__tests__/interrupt-bridge.test.ts` has current behavior test "errors with descriptive message when chunk has no payload" at lines around 367-379.
- Proposed fail-first test: use `makeLocalMastraAgent({ streamChunks: [{ type: "data-workspace-metadata", data: { toolName: "code-explore" } }, { type: "text-delta", payload: { text: "ok" } }] })`; assert no thrown error and one text chunk `ok`.
- Runner command:
  ```bash
  cd /tmp/ag-ui-scout
  pnpm install --frozen-lockfile
  pnpm --filter @ag-ui/mastra test -- src/__tests__/interrupt-bridge.test.ts
  ```
- Before upstream PR: run targeted fail-before/pass-after in runner, then `pnpm --filter @ag-ui/mastra test`, `pnpm --filter @ag-ui/mastra typecheck`, and fresh duplicate search.

## 6-Subagent Synthesis

- Bug-signal: initial fresh LangGraph leads were mostly duplicate-covered; `#1635` is the cleanest small adapter bug with direct agent UI stream impact.
- Duplicate-race: high repo-level PR pressure. Exact `#1635` duplicate not found, but related Mastra risks include broad [`#1253`](https://github.com/ag-ui-protocol/ag-ui/pull/1253), adjacent [`#1727`](https://github.com/ag-ui-protocol/ag-ui/pull/1727), and `text-end` issue/PR [`#836`](https://github.com/ag-ui-protocol/ag-ui/issues/836)/[`#837`](https://github.com/ag-ui-protocol/ag-ui/pull/837).
- Process: assignment-first. No cold PR. No upstream comment now because the issue already contains repro/root cause and a "happy to open PR" ask; another comment without runner evidence adds little.
- Repro: secret-free unit-level Mastra harness is feasible. Dedicated runner preferred because `pnpm install` in Nx monorepo is dependency-heavy.
- Patchability: small if scoped narrowly to tolerate payload-less custom chunks; broad if trying to cover all Mastra 1.31 chunk semantics.
- PR-readiness: not `PR-READY`. Missing runner repro, duplicate recheck, assignment/approval.

## 3-Subagent Critique

- Factology/duplicates: `#1635` is open, unassigned, unlabeled, uncommented; current `main` still has fatal missing-payload guard. No exact PR duplicate found. Keep overclaim narrow: fatal `data-workspace-metadata` abort is the root bug; other chunk-type warnings are secondary.
- Process gates: assignment-first and runner-repro-needed block `PR-READY`. Internal `CANDIDATE` is acceptable only with regression card and blockers recorded.
- Actionability: internal `CANDIDATE`; do not post upstream now. Unblock requires runner repro, fresh duplicate gate over `#1253/#1727/#836`, and maintainer assignment or explicit PR welcome.

## Unblock Events

1. Run targeted `#1635` fail-first repro in `corp-opensource-runner` or documented equivalent.
2. Re-run duplicate gate for `#1635`, `data-workspace-metadata`, `missing payload`, `Malformed stream chunk`, `Mastra 1.31`, and related `#1253/#1727/#836`.
3. If repro holds and duplicate gate stays clean, draft upstream assignment/approval comment before code or PR.
4. Only after maintainer approval/assignment, prepare minimal patch and tests; do not broaden to full Mastra 1.31 chunk semantics without maintainer direction.

## Refresh 2026-05-27 21:07 UTC

Follow-up note: [AG-UI / Mastra candidate refresh](ag-ui-mastra-candidate-refresh-2026-05-27.md).

Live refresh kept `#1635` open, unassigned, unlabeled and uncommented. Exact duplicate PR still not found for `data-workspace-metadata` / payload-less custom chunks, but near-collision set is broader than the original note: `#1253`, `#837`, `#1727`, and `#1468`.

Updated decision: `COMMENT-FIRST` after runner-backed test card. Do not post upstream and do not open PR in this cycle; first prove the secret-free `@ag-ui/mastra` unit regression in runner, then ask maintainers whether a narrow payload-less custom chunk fix should land separately or fold into the broader Mastra adapter work.
