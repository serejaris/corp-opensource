# opencode #29697 IME TUI stale cells watch

Дата: 2026-05-28

Tracker:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561938155

Статус: `WATCH`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Fresh discovery после `2026-05-28T07:55:00Z` поднял `anomalyco/opencode#29697`: https://github.com/anomalyco/opencode/issues/29697

Issue reports that macOS Terminal.app CJK IME composition corrupts the opencode TUI layout: during composition the prompt/right UI shifts, and after committing the character most of the layout snaps back but stale cells remain past the drawable area. Reporter attached screenshots and environment details: OpenCode `1.15.10`, macOS Sequoia `15.7.5`, Terminal.app, Chinese Pinyin IME.

## Parent live gates

- Issue: open, assigned to `simonklee`, comments `0`, labels отсутствуют.
- Created: `2026-05-28T07:49:50Z`; updated: `2026-05-28T07:50:55Z`.
- Exact upstream PR/comment в этом цикле не делались.
- Runner `#10` всё ещё unavailable; local macOS Terminal.app / IME repro невозможен в текущем Linux workspace.

## Duplicate / race

Focused search/list signals:

- `gh issue list -R anomalyco/opencode --search "IME composition Terminal.app TUI stale cells"` found only `#29697`.
- `gh issue list -R anomalyco/opencode --search "Chinese IME TUI macOS"` found adjacent IME/input issues `#28118`, `#16310`, and `#19305`, but not exact stale-cell/diff-render cover.
- `gh pr list -R anomalyco/opencode --search "IME composition TUI layout diff render"` returned no exact PR cover.

Risk remains coordination-sensitive because the issue is already assigned.

## 6-role synthesis

Новые subagent threads недоступны из-за thread limit, поэтому цикл переиспользовал шесть уже перечисленных agents.

- Fresh discovery ranked `opencode#29697` as a strong TUI lead with visible screenshots and low exact-duplicate signal.
- Duplicate/race critique found no exact issue/PR cover, but flagged the assignee as a maintainer-owned race.
- Actionability critique downgraded it below `opencode#29694`: macOS Terminal.app + CJK IME + OpenTUI diff rendering is repro-heavy and not runner-friendly until a macOS/IME path exists.
- Process critique blocks upstream comment/PR: no recorded runner/manual repro, issue is assigned, and upstream action would need fresh exact search plus a useful source-backed narrowing.
- Active-watch refresh found no material state changes after the `07:55Z` cutoff.

## Decision

`next_status: WATCH`

Трекать `opencode#29697` как assigned IME/TUI watch. Re-entry условия: assignee inactivity, maintainer request for help, source-backed narrowing of the diff-render invalidation path, or reliable macOS Terminal.app IME repro evidence.
