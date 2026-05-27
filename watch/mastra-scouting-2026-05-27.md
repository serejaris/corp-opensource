# mastra-ai/mastra scouting, 2026-05-27

Internal tracker: [#58](https://github.com/serejaris/corp-opensource/issues/58). Umbrella: [#52](https://github.com/serejaris/corp-opensource/issues/52).

Final `next_status`: `CANDIDATE`.

Primary lane: [`mastra-ai/mastra#17118`](https://github.com/mastra-ai/mastra/issues/17118) — Mastracode slash command picker truncates long command descriptions instead of wrapping.

## Итог

`mastra-ai/mastra` подходит для Paperclip-like / AI-native scouting: 24k+ stars, активный TypeScript monorepo, agent/workflow/MCP/tooling surface, свежий release `@mastra/core@1.37.0` от 2026-05-27. Риск duplicate-race высокий: сотни открытых PR/issues, быстрый maintainer/bot triage, changed-test gate и обязательный linked issue для PR.

В этом цикле выбран один internal candidate: `#17118`. Upstream PR/comment не делались.

## Process gates

- 6-subagent scouting: выполнен.
- Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow`: недоступны в текущем окружении; использован documented fallback через live GitHub/`gh`, shallow checkout и multi-agent triage.
- Parent live gates: repo state, issue/PR state, assignee/labels, linked PRs/timeline, PR search, contribution docs, toolchain/test gates проверены.
- 3-subagent critique: фактология/дубликаты, process gates, actionability выполнены; все роли подтвердили `CANDIDATE` для `#17118`.
- Runner repro: не запускался; это блокер для `PR-READY`, но не для `CANDIDATE`.
- Upstream action: 0 PR, 0 comment.

## Parent live gates

- Repo: `mastra-ai/mastra`.
- Default branch: `main`.
- Latest release: `@mastra/core@1.37.0`, 2026-05-27.
- License: Apache-2.0 outside `ee/`; enterprise license inside `ee/`.
- Contribution docs: `CONTRIBUTING.md`, `DEVELOPMENT.md`, PR template, issue templates present.
- Process: PR must link related issue (`Fixes #123` / `Closes #123`) or may be closed; changed-test gate expects focused tests that fail on base.
- Toolchain: Node `>=22.13.0`, `pnpm@10.29.3`, Turbo, Vitest. Relevant command for this lane: `pnpm --filter ./mastracode exec vitest run src/tui/components/__tests__/wrapping-select-list.test.ts`.

## Candidate: `#17118`

Issue state:

- Open, unassigned.
- Labels: `bug`, `mastracode`, `impact:medium`, `effort:medium`, team labels.
- Timeline: only triage-bot comment visible.
- Duplicate gate: PR search for `17118`, slash-command picker, long descriptions, Mastracode truncation returned no direct open/closed PR.

Code/test surface from current `main`:

- `mastracode/src/tui/setup.ts` builds slash-command descriptions and passes them to `CombinedAutocompleteProvider`.
- `mastracode/src/tui/components/wrapping-select-list.ts` already wraps long item labels and has item-to-item navigation behavior.
- `mastracode/src/tui/components/__tests__/wrapping-select-list.test.ts` already contains focused rendering/width/navigation tests.

Regression card draft:

- Contract: long slash-command descriptions wrap inside the picker instead of being clipped/truncated to one row.
- Test file: likely `mastracode/src/tui/components/__tests__/wrapping-select-list.test.ts` or a new focused test around slash-command autocomplete rendering.
- Command: `pnpm --filter ./mastracode exec vitest run src/tui/components/__tests__/wrapping-select-list.test.ts`.
- Required before PR: fail-before/pass-after evidence on current `main` or controlled pre-fix path; repeat duplicate gate for `#17118` and overlapping Mastracode TUI/autocomplete PRs.

## Side lanes

| Lane | Status | Evidence |
|---|---|---|
| `#17137` storage debug spam on `agent.generate()` | `WATCH` | Strong key-free MRE and no direct PR found, but core/workflow/storage fix shape is ambiguous: propagate Mastra ref, suppress debug, or avoid storage path. Keep as backup after duplicate recheck. |
| `#16957` `mastra migrate` install bypasses immutable policy | `COMMENT-FIRST / WATCH` | High impact and no direct PR found, but broad CLI/deployer dependency-install contract. Needs runner-verified Yarn immutable repro and maintainer preference before code. |
| `#17094` `BatchPartsProcessor` drops final part | `WATCH` | Strong MRE, but assigned to maintainer `mikhael28`; no racing PR. |
| `#16444` MCP const-constrained tool fields | `COMMENT-FIRST / WATCH` | Good MCP/schema lane, but status waits for author/MRE and maintainer asked reporter about PR. |

## Duplicate-covered / no-go

- `#17115` covered by open `#17120`.
- `#17119` covered by open `#17155`.
- `#17081` covered by open `#17088`.
- `#17076` covered by open `#17130`.
- `#17075` covered by open `#17134`.
- `#16408` covered by open `#16448`.
- `#16383` covered by open `#16616`; schema-adjacent conflict risk also exists around `#17052`.

## Critique synthesis

Factology/duplicates: `#17118` has no direct PR; related wrapping work is precedent, not duplicate. Most other high-signal lanes are already covered by active PRs.

Process gates: `CANDIDATE` is safe. It is not `PR-READY` until runner/local fail-before/pass-after evidence and a repeat duplicate gate are recorded.

Actionability: `#17118` is the cleanest single lane because it is narrow, unassigned, secret-free, and has a plausible focused test surface. No upstream comment now; issue is already accepted into triage and has no design ambiguity.

## Watch triggers

- `#17118` gets an assignee or direct PR.
- Maintainer comments with preferred implementation or asks for PR/tests.
- Runner/unit repro does not reproduce on current `main`.
- A broad Mastracode TUI/autocomplete PR appears and overlaps the picker path.
- `#17137` gets maintainer clarification and `#17118` becomes occupied.
