# AG-UI / Mastra candidate refresh, 2026-05-27

Trackers: [#56](https://github.com/serejaris/corp-opensource/issues/56), [#58](https://github.com/serejaris/corp-opensource/issues/58), umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52).

Итог:

- `ag-ui-protocol/ag-ui#1635`: `COMMENT-FIRST` after runner-backed test card; no PR and no upstream comment in this cycle.
- `mastra-ai/mastra#17118`: `CANDIDATE`; no PR and no upstream comment in this cycle.

## Mastra follow-up 23:23 UTC

Bounded refresh по `mastra-ai/mastra#17118`: parent live gates через `gh`, 6 read-only subagents, затем 3-role critique. Upstream action count: `0`.

Live gates:

- Issue `#17118` остаётся open и unassigned; labels: `bug`, `trio-tb`, `trio-wp`, `impact:medium`, `effort:medium`, `mastracode`.
- Triage comment от `daneatmastra` просит MRE where possible. Это maintainer interest, но не assignment и не разрешение на PR.
- Exact PR search по `17118`, `slash-command`, `slash command`, `autocomplete`, `Mastracode`, `terminal width`, `terminal-width` не нашёл covering PR.
- Adjacent `#17002` closed после `#17005`; это `ask_user` option label crash/wrapping, не slash-command descriptions.
- Adjacent `#17054` merged; он добавил ask_user picker option label wrapping with tests, но был merged до `#17118` и не является exact coverage для slash-command descriptions через `CombinedAutocompleteProvider` / `SelectList`.
- Adjacent `#17083` open, `DIRTY/CONFLICTING`, broad Mastracode ask_user multi-select PR. Это overlap/conflict risk по TUI components, но не exact functional duplicate.

6-role synthesis:

- Factology/status: `#17118` остаётся свежим open bug candidate в активном repo.
- Duplicate/adjacent: exact duplicate low; adjacent overlap medium из-за `#17054/#17083`.
- Process: Mastra PR flow требует linked issue, clear problem/solution, tests и MRE-quality evidence. Triage/MRE request не является assignment нам.
- Runner/repro: нет runner-backed fail-before evidence, terminal-width fixture, expected/actual screenshot/text capture или regression card.
- Tracker: обновить README, этот watch note и issue `#58`; не трогать базовый `mastra-scouting`, если статусы не расходятся.
- Final synthesis: ровно один status — `CANDIDATE`.

3-role critique:

- Factology/duplicates: approve с caveat не писать `PR-READY`; `#17083` — overlap/conflict risk, не duplicate.
- Process gates: approve internal-only update; upstream comment/PR запрещены до runner MRE + regression card + fresh duplicate gate.
- Actionability: issue `#58` comment justified because evidence changed materially, even though status remains `CANDIDATE`.

Decision: keep `next_status: CANDIDATE`.

Re-entry condition: move to `COMMENT-FIRST` only after runner-backed MRE, fresh linked PR/issue search, and 3-role critique; move to `PR-READY` only after fail-before/pass-after regression card, concrete terminal-width fixture, target test command, and process gate.

Runner: next gate is secret-free Mastracode slash-command picker repro in `corp-opensource-runner`, with fixed terminal width, long command description, actual truncation, expected wrapping behavior, and targeted test path if reproducible. No local heavy repro in this cycle.

## Parent live gates

### `ag-ui-protocol/ag-ui#1635`

- Repo live state at `2026-05-27 21:07 UTC`: active, not archived/fork, default branch `main`, MIT, `13879` stars, `1243` forks, `173` issues, `130` PRs.
- Latest release: `release/2026-05-27`, published `2026-05-27 18:57 UTC`.
- Issue state: open, unassigned, no labels, no comments, not updated since creation on `2026-05-05`.
- Issue contract: `@ag-ui/mastra` should not abort the stream on payload-less Mastra custom chunks such as `data-workspace-metadata`; later text chunks should still reach AG-UI.
- Process gate: AG-UI `CONTRIBUTING.md` is issue-first and assignment-first. It asks contributors to reach out before significant work, ask to be assigned, coordinate with code owners, and include `Fixes #<issue-number>` in PRs.
- Runner gate: secret-free unit repro is feasible for `@ag-ui/mastra`, but current local env is not enough for repo work (`node v20.19.2`, no `pnpm`). Use dedicated runner with Node 22/24 and pnpm 10.

Duplicate/PR gate:

- No exact PR found for `data-workspace-metadata`, `Malformed stream chunk`, `missing payload`, or payload-less custom chunk handling.
- Near collisions to record before any upstream action: broad Mastra adapter rework `#1253`, `text-end` issue/PR `#836/#837`, output-processor issue/PR `#1726/#1727`, and stream boundary PR `#1468`.
- Duplicate risk: exact duplicate low, same-file/architecture collision medium-high.

Decision: downgrade action mode from internal `CANDIDATE` to `COMMENT-FIRST-after-runner-evidence`. The first upstream action, if repro holds, should be a short maintainer question asking whether a narrow payload-less custom chunk fix is wanted separately or should be folded into the broader Mastra adapter work. No cold PR.

### `mastra-ai/mastra#17118`

- Repo live state at `2026-05-27 21:07 UTC`: active, not archived/fork, default branch `main`, `24415` stars, `2142` forks, `232` issues, `217` PRs.
- Latest release: `@mastra/core@1.37.0`, published `2026-05-27 16:19 UTC`.
- License gate: GitHub reports `Other/NOASSERTION`; repo `LICENSE.md` maps ordinary code to Apache-2.0 and `ee/` directories to Mastra Enterprise License. This lane should avoid `ee/`.
- Issue state: open, unassigned; labels `bug`, `trio-tb`, `trio-wp`, `impact:medium`, `effort:medium`, `mastracode`; one triage comment asks for an MRE where possible.
- Issue contract: long Mastra Code slash-command descriptions should wrap inside the picker instead of being clipped/truncated to one row.
- Process gate: PRs must link an issue and include focused tests; development requires Node `>=22.13.0`, pnpm `>=10.18.0`, and changeset for code changes.
- Runner gate: secret-free test is plausible in `mastracode`, but current local env is not enough (`node v20.19.2`, no `pnpm`). Use dedicated runner with Node 22+ and pnpm 10.

Duplicate/PR gate:

- No exact PR or issue found for slash-command picker descriptions / `SelectList` / `CombinedAutocompleteProvider` wrapping.
- Near precedent/collision: closed `#17002`, merged `#17005`, merged `#17054` introducing `WrappingSelectList` for `ask_user`, and open `#17083` around ask_user multi-select. These are not exact duplicates but shape the expected implementation.
- Duplicate risk: low-medium.

Decision: keep `CANDIDATE`, not `PR-READY`. Next admissible step is runner-backed current-main repro or focused test discovery around slash-command autocomplete rendering, then repeat duplicate gate.

## 6-subagent synthesis

1. Repo fit/activity: both repos remain high-fit and active. AG-UI is narrower for protocol/integration stream semantics; Mastra is broader and high-churn.
2. Issue quality: `ag-ui#1635` has a stronger root-cause contract and reporter evidence. `mastra#17118` is narrower UI/TUI work but lacks concrete command, terminal width and reproducible fixture.
3. PR/duplicate risk: AG-UI has no exact duplicate but several same-file Mastra adapter PRs; Mastra has no exact duplicate and useful wrapping precedent.
4. Process/license/community: AG-UI is assignment-first and should not receive a cold PR. Mastra allows PRs with linked issues/tests, but MRE/repro is still expected.
5. Runner/repro: both are secret-free in principle, but current local env lacks required Node/pnpm. Dedicated runner should be used before upstream action.
6. Actionability: AG-UI should become `COMMENT-FIRST-after-runner-evidence`; Mastra remains `CANDIDATE`.

## 3-subagent critique

- Factology/duplicates: do not claim either lane is fixed or PR-ready. Record AG-UI near collisions `#1253/#837/#1727/#1468`; record Mastra wrapping precedent `#17002/#17005/#17054/#17083`.
- Process gates: AG-UI assignment/code-owner alignment blocks PR and makes the next upstream mode comment-first only after local/runner proof. Mastra needs linked issue, tests, changeset, and MRE-quality evidence before PR.
- Actionability: no upstream comment in this cycle because neither lane has fresh runner evidence. Internal update only.

## Next gates

1. For `ag-ui#1635`, run a secret-free `@ag-ui/mastra` unit repro in runner: payload-less custom chunk followed by text chunk must not call `onError` or abort.
2. For `mastra#17118`, run a secret-free Mastra Code TUI/autocomplete repro in runner with fixed terminal width and a long slash-command description.
3. Re-run duplicate gates immediately before any upstream comment or PR.
4. If runner evidence holds, run another 3-subagent critique before posting upstream.

## Cross-lane correction heartbeat - 2026-05-28 01:14 UTC

Six-lane readiness refresh plus 3-role critique kept both lanes internal-only and made two corrections.

- `ag-ui-protocol/ag-ui#1635`: issue remains open with no exact PR. `#1727` is adjacent output-processor work, not an exact payload-less custom chunk fix. Final internal status remains `CANDIDATE`; the admissible future upstream mode is comment-first only after runner-backed `@ag-ui/mastra` evidence and assignment/code-owner gate.
- `mastra-ai/mastra#17118`: issue remains open/labeled, and triage still asks for an MRE. Merged `#17054` is more relevant than a generic adjacent PR because it added wrapping for `ask_user` picker option labels; it is still not proven to cover slash-command description wrapping, but future notes must treat it as a partial/precedent gate. Open `#17083` remains broad Mastracode overlap.

Runner gate is unchanged: local Node is `v20.19.2`, `pnpm` is absent, and `corp-server` does not resolve. No upstream comment/PR was made; upstream action count `0`.
