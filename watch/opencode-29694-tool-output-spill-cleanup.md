# opencode #29694 tool-output spill cleanup

Дата: 2026-05-28

Tracker:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561885928

Статус: `WATCH`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Fresh discovery после `2026-05-28T07:45:00Z` поднял `anomalyco/opencode#29694`: https://github.com/anomalyco/opencode/issues/29694

Issue сообщает, что большие truncated tool-output spill files в `~/.local/share/opencode/tool-output` не чистятся автоматически и могут занимать десятки гигабайт. Reporter показал `63G` directory size и multi-GB `tool_*` files, нашёл в source `packages/opencode/src/tool/truncate.ts` cleanup path с `RETENTION = Duration.days(7)`, но не нашёл caller через `rg`.

## Parent live gates

- Issue: open, assigned to `nexxeln`, comments `0`, labels отсутствуют.
- Created: `2026-05-28T07:31:37Z`; updated: `2026-05-28T07:32:34Z`.
- Upstream PR/comment в этом цикле не делались.
- Runner `#10` всё ещё unavailable; local/heavy checkout/repro не запускался.

## Duplicate / race

Open PR list не показывает exact cleanup/prune cover для `tool-output` spill files.

Search/list signals:

- `gh issue list --search "tool-output OR truncate OR cleanup OR spill"` нашёл сам `#29694` и related/adjacent state/cache/truncation issues, но не exact cleanup bug.
- `gh pr list --search "tool-output cleanup truncate"` нашёл только `#26167` (`retry empty stream truncations with attempt cap`), не cleanup/prune cover.
- `#29694` уже assigned to maintainer/contributor `nexxeln`, поэтому не открывать cold PR и не комментировать без source-backed current-main evidence или maintainer signal.

## 6-role synthesis

Новые subagent threads недоступны из-за thread limit, поэтому цикл переиспользовал шесть уже перечисленных agents.

- Fresh discovery выделил `opencode#29694` как самый практичный fresh issue после CLIProxyAPI downgrade.
- Duplicate/race critique: exact PR cover не найден, но assignee делает lane coordination-sensitive.
- Actionability critique: высокий AI-agent runtime value и компактный likely fix surface, но статус `WATCH / CANDIDATE-if-stale-or-maintainer-signal`.
- Process critique: runner unavailable, GitHub Search API частично rate-limited earlier, upstream action не делался.
- Synthesis status for bounded cycle: `WATCH`.

## Decision

`next_status: WATCH`

Трекать `opencode#29694` как high-value assigned watch. Re-entry условия: assignee inactivity, maintainer request, или runner/source-backed evidence, что cleanup всё ещё unwired на current main и exact PR не существует.
