# Docker MCP scope check, 2026-05-27

Tracker: [#65](https://github.com/serejaris/corp-opensource/issues/65), umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52)

Итоговый `next_status`: `WATCH`.

Combined Docker MCP scope остаётся только из-за узкой watch-lane по `docker/mcp-gateway`. `docker/mcp-registry` закрыт как generic scouting surface и остаётся только reference / exact metadata-validation target по конкретной карточке.

## Parent live gates

### `docker/mcp-gateway`

- Repo: created `2025-04-22`, default branch `main`, not archived/fork, MIT, no latest release.
- Activity: `1411` stars, `244` forks, `79` issues, `34` PRs; pushed `2026-05-27 18:40 UTC`, updated `2026-05-27 18:40 UTC`.
- Community/process: community profile `87%`; README, CONTRIBUTING, CODE_OF_CONDUCT, LICENSE and PR template present. CONTRIBUTING lists `make docker-mcp`, `make test`, `make lint`, `make integration`.
- Fit: useful MCP distribution/gateway runtime surface for validation/config/session/secrets bugs, but not a broad PR-hunting lane.

Issue-level watch leads, not candidates:

- `#442` nil `req.Session` panic in `mcp-add`: narrow crash signal, but only a watch lead. Current PR search did not find a direct `req.Session nil` PR; nearby risk remains from `#368` and `#381`.
- `#430` config schema validation: good exact-validation shape, but duplicate risk is high due `#462` and older schema PRs `#234/#409`.
- `#393` custom catalog `server inspect`: possible parser/catalog validation lead, needs linked-PR/artifact check.
- `#433/#463` stale secrets/config refresh: real runtime consistency watch, but higher Desktop/session/secrets repro risk and overlap risk with `#384`, `#338`, `#359`, `#379`.

Blocked/demoted lanes:

- Feature/design items such as `#492`, `#474`, `#378`, `#370` need maintainer direction, not opportunistic PR.
- Windows/WSL/proxy/Desktop issues such as `#424`, `#440`, `#457` are high-cost repro lanes.
- OAuth/secrets/client-integration issues must avoid public security-sensitive handling unless the path is clearly non-vulnerability and reproducible.

### `docker/mcp-registry`

- Repo: created `2025-06-09`, default branch `main`, not archived/fork, MIT, no latest release.
- Activity: `492` stars, `819` forks, `62` issues, `496` PRs; pushed `2026-05-27 17:27 UTC`, updated `2026-05-27 18:34 UTC`.
- Community/process: community profile `87%`; README, CONTRIBUTING, CODE_OF_CONDUCT, LICENSE, PR template and issue template directory present.
- Fit: official registry/catalog reference, but current public queue is dominated by add-server, metadata, policy and server-specific support.
- PR queue: first open PR page is mostly add-server submissions and bot pin updates. `#3302` already has open PR `#3800`.

Decision: `NO-GO` for generic scouting scope. Do not hunt arbitrary add-server/catalog PRs here. Reopen only for exact metadata-validation with a concrete linked issue, low duplicate risk and runner-safe repro.

## 6-subagent synthesis

1. Repo fit/activity: gateway has narrow Paperclip-like value as MCP gateway/runtime; registry is official and active but lower generic contribution fit.
2. Issue quality: gateway has watch leads around panic/config/schema/session consistency; registry issues are mostly catalog requests, policy or server-specific support.
3. PR/duplicate risk: gateway has nearby stale/conflicting PRs around config, hot reload, secrets and panic paths; registry has very high queue noise and existing overlap for SSL_VERIFY.
4. Process/community: both repos pass MIT/community gates, but Docker team review and security-sensitive ownership make narrow scope mandatory.
5. Runner/repro: Docker MCP repro is blocked in the current environment; `docker`, `go` and `task` are absent from PATH. Dedicated runner with Docker runtime/buildx and Go is required before candidate promotion.
6. Actionability: combined status is `WATCH`; gateway retained as exact-validation watch, registry generic scope closed.

## 3-subagent critique

- Factology: do not call gateway leads candidates. Write only "watch leads" until issue-level duplicate gate and runner repro exist.
- Duplicates: for `#442`, say only that a direct `req.Session nil` PR was not found in this check; nearby crash/false-success PRs remain. For `#430`, mention `#462` and older schema PRs. For `#433/#463`, mention `#384` and secret-related PRs.
- Process: watch note and tracker are required before commit; no upstream comment/PR is authorized from this cycle.
- Actionability: singular next step is a separate narrow gate for one gateway issue, preferably `#442` or `#430`, if a future candidate cycle is needed.

## Runner note

No repro/tests were run. This was read-only triage. The current environment cannot run Docker MCP checks because `docker`, `go` and `task` are not available in PATH. Per repo rules, Docker MCP repro should wait for `corp-opensource-runner` provisioning or an approved dedicated runner path. No secrets should be placed in this repo.

## Final decision

- `docker/mcp-gateway`: `WATCH`, exact-validation/config only; no generic PR hunting.
- `docker/mcp-registry`: `NO-GO` for generic scope; reference / exact metadata-validation only.
- Upstream action: none.
