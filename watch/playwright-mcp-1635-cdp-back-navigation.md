# Playwright MCP #1635 CDP back navigation timeout

Date: 2026-05-27

## Links

- Upstream issue: https://github.com/microsoft/playwright-mcp/issues/1635
- Internal tracker: https://github.com/serejaris/corp-opensource/issues/36
- Cycle comment: https://github.com/serejaris/corp-opensource/issues/36#issuecomment-4557472301
- Existing upstream comment-first: https://github.com/microsoft/playwright-mcp/issues/1635#issuecomment-4552993898

## Status

`WATCH / COMMENT-FIRST already done`.

This is a strong Paperclip-like browser/MCP runtime candidate, but it is not `PR-READY`. `microsoft/playwright-mcp` contribution policy requires a linked issue plus maintainer approval or assignment before a PR. The upstream issue has no label, assignee, or maintainer response as of the 2026-05-27 18:25 UTC parent gate.

## Live Gates

| Gate | Result |
|---|---|
| Wrapper repo | `microsoft/playwright-mcp`: 33101 stars, 2718 forks, latest release `v0.0.75`, pushed 2026-05-23, 6 open issues, 4 open PRs. |
| Core repo | `microsoft/playwright`: 89637 stars, latest release `v1.60.0`, pushed 2026-05-27, 143 open issues, 22 open PRs. |
| Upstream issue | `#1635` open, created 2026-05-27 08:19 UTC, no labels, no assignee, no maintainer response. |
| Evidence | Reporter showed direct `connectOverCDP` repro: default and `domcontentloaded` `page.goBack()` time out after URL commits; `waitUntil: "commit"` succeeds. |
| Search / duplicates | Direct open PR list in `playwright-mcp` has only docs/deps/test snapshot PRs. Direct issue search found only `#1635`. Search API was intermittently rate-limited/403, so do not claim duplicate-clean. |
| Contribution gate | `CONTRIBUTING.md` says unsolicited PRs without linked issue plus approval/assignment will be closed; MCP core moved to `microsoft/playwright`. |
| Runner gate | Not run. Browser/CDP repro should run in `corp-opensource-runner` or documented stable browser runner before any PR-ready claim. |

## 6-Role Synthesis

| Role | Finding |
|---|---|
| repo-fit | High repo fit: canonical browser automation MCP server, compact issue queue, direct relevance to agent browser workers. Work mode should be issue-first/comment-first, not PR-first. |
| bug-signal | `#1635` is the strongest lane: narrow, fresh, user-visible, and backed by direct Playwright CDP evidence. `#1636` is high-impact but needs long-running repro. |
| repro-path | Secret-free repro exists with Chromium/Chrome CDP endpoint and local two-page history. MCP-level test should drive `browser_navigate`, `browser_navigate`, `browser_navigate_back`. |
| patchability | Likely source is `microsoft/playwright`, not wrapper repo: `packages/playwright-core/src/tools/backend/navigate.ts`. Minimal MCP-only patch may use `waitUntil: "commit"` for back/forward, but maintainers may prefer deeper core navigation behavior. |
| duplicate-race | No exact competing PR found via direct lists/timeline. Search API rate-limit means duplicate-clean is not proven. `#1636` overlaps resource lifecycle issues `#1634/#41009/#41017`, so it stays separate watch. |
| PR-readiness | Not PR-ready: no maintainer approval/assignment, no runner-backed repro, incomplete duplicate search, and no regression card with pre-fix fail. |

## Regression Card Draft

- Contract: `browser_navigate_back` over a CDP-attached page should complete once history traversal commits the previous URL, rather than timing out while waiting for `load`/`domcontentloaded`.
- Likely test file: `microsoft/playwright` `tests/mcp/cdp.spec.ts`.
- Likely source file: `packages/playwright-core/src/tools/backend/navigate.ts`.
- Candidate command, to verify in runner: `npm run ctest-mcp -- cdp.spec.ts -g "navigate back"`.
- Expected pre-fix failure: MCP `browser_navigate_back` times out even though the page URL has changed back.
- Expected post-fix pass: tool succeeds and snapshot/URL reflect the previous page.

## Decision

Final `next_status`: `WATCH`.

`COMMENT-FIRST` has already been done upstream. Next unblock event is maintainer approval/assignment or a maintainer question. If that happens, rerun live gates, repeat duplicate search, and run a runner-backed CDP repro before moving toward `PR-READY`.
