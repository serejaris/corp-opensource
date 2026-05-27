# AG-UI / Mastra candidate refresh, 2026-05-27

Trackers: [#56](https://github.com/serejaris/corp-opensource/issues/56), [#58](https://github.com/serejaris/corp-opensource/issues/58), umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52).

Итог:

- `ag-ui-protocol/ag-ui#1635`: `COMMENT-FIRST` after runner-backed test card; no PR and no upstream comment in this cycle.
- `mastra-ai/mastra#17118`: `CANDIDATE`; no PR and no upstream comment in this cycle.

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
