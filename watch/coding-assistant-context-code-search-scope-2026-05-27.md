# Coding assistant context / code-search scope, 2026-05-27

Tracker: [#80](https://github.com/serejaris/corp-opensource/issues/80)

## Scope

Repo-scope scouting по code context, spec-driven workflows, semantic code search and editor/runtime reliability:

- `safishamsi/graphify`
- `Fission-AI/OpenSpec`
- `TabbyML/tabby`
- `github/CopilotForXcode`
- `probelabs/probe`

Обязательный skill `open-source-bug-scouting` в окружении не установлен, поэтому цикл выполнен documented fallback: 6 read-only subagents, parent live gates через GitHub API/`gh`, затем internal 3-role critique. Upstream comments/PR не создавались.

## Parent Live Gates

| Repo | Live signal, 2026-05-27 | Issue/PR pressure | Process / runner gate |
|---|---|---|---|
| `safishamsi/graphify` | MIT, Python, default branch `v8`, `54,969` stars, pushed `2026-05-27`, latest `v0.8.21` on `2026-05-27` | API parent count `266`, open PR count `156`. Fresh lanes include `#1043`, `#1037`, `#1033`, but `#1033` has PR `#1038` and language/extractor lanes are crowded. | No full `CONTRIBUTING.md`; README + `SECURITY.md` + `AGENTS.md`. Tests via `pytest tests/ -q`. Strong fit but suspicious star-spike/provenance and high duplicate-race risk. |
| `Fission-AI/OpenSpec` | MIT, TypeScript, default branch `main`, `51,174` stars, pushed `2026-05-27`, latest `v1.3.1` on `2026-04-21` | API parent count `356`, open PR count `103`. `#1112` has PR `#1113`; `#1129/#1103` overlap `#1133`; Mistral Vibe has several competing PRs. | README contribution section, no root `CONTRIBUTING.md`/`SECURITY.md` found. Tests via `pnpm install`, `pnpm run build`, `pnpm test`, lint/tsc/changeset in CI. |
| `TabbyML/tabby` | Rust, GitHub license `NOASSERTION`; root license says Apache-2.0 for non-`ee/`, separate `ee/` license. `33,548` stars, pushed `2026-03-02`, latest `v0.32.0` on `2026-01-25` | API parent count `315`, open PR count `66`. `#4486` has PR `#4487`; git/private repo sync lanes have several active PRs; `#4491` is severe but fuzzy and platform/repro-heavy. | Strong `CONTRIBUTING.md`; Rust + pnpm monorepo. Meaningful server/model/repo-provider repro often needs Docker, GPU, provider credentials, or mocked service paths. |
| `github/CopilotForXcode` | MIT, Swift, official GitHub repo, `6,109` stars, pushed `2026-05-20`, latest `0.50.0` on `2026-05-20` | API parent count `227`, open PR count `2`. Issues are noisy/spam-heavy; real bugs such as `#812/#811` need macOS/Xcode/auth evidence. | PR template and auto-close workflow say external contributions are not accepted. Treat as `NO-GO` for PR unless maintainers explicitly invite; Linux runner cannot close app-level repro. |
| `probelabs/probe` | Apache-2.0 root license, Rust, default branch `main`, `616` stars, pushed `2026-05-21`, latest `v0.6.0-rc319` on `2026-05-21` | API parent count `15`, open PR count `2`. Best lane: `#568`, open/unassigned, labels `bug` + `external`; no exact open PR found for `symbols` JSON stdout pollution. | `CONTRIBUTING.md`, `SECURITY.md`, Code of Conduct and tests exist. `Cargo.toml` says `license = "MIT"` while root license is Apache-2.0; note before PR. Full LSP suite needs extra deps; targeted repro can be runner-light. |

## 6-subagent synthesis

1. Repo fit/activity: `probelabs/probe` ranked first for clean AI-native code-search/MCP fit with small backlog. `OpenSpec` and `graphify` are high-fit but have star-spike/process/duplicate concerns. `Tabby` is mature but slower and heavier. `CopilotForXcode` is vendor-owned and contribution-blocked.
2. Issue actionability: `probelabs/probe#568` is the best lane. It reports `symbols` JSON output polluted by debug/progress stdout, causing Node wrapper JSON parse failure. Other lanes were PR-covered, fuzzy, or too platform/service heavy.
3. Duplicate/process: no exact open PR for `probe#568`; open PRs are `#561` and `#264`. Graphify/OpenSpec lanes have high overlap; Tabby client/git lanes have active PRs; CopilotForXcode is process-blocked.
4. Community/process: Probe has the strongest contribution/test process for a narrow bugfix, with a license metadata mismatch to record. CopilotForXcode is `NO-GO` for external PRs. Graphify/OpenSpec have weaker formal process docs.
5. Runner/repro: `probe#568` is secret-free and should run in `corp-opensource-runner`, capturing exact npm/CLI version, command, stdout/stderr split, exit code and JSON parse failure. Full LSP tests may require `gopls`/TypeScript/PHP tooling.
6. Final synthesis: promote `probelabs/probe#568` to internal `CANDIDATE`, not `PR-READY`; no upstream comment without our own repro because maintainer/Visor already asked for exact repro details.

## 3-role critique

- Фактология/дубликаты: `#568` is open/unassigned, no exact open PR found, but closed PRs `#205` and `#109` are precedent for stdout/JSON pollution. Current source caveat: `src/extract/symbols.rs::handle_symbols` appears to print clean JSON, while `Pattern:`/`Path:` strings are in `src/main.rs::handle_search`. Exact path is not yet proven.
- Process gates: internal `CANDIDATE` tracking is safe. Upstream PR/comment is not safe before runner repro and repeat duplicate search. Probe has tests and contribution docs; root `LICENSE` vs `Cargo.toml` license mismatch should be noted before PR.
- Actionability: choose exactly `CANDIDATE`. Next step is fail-before repro in `corp-opensource-runner`, not upstream comment.

## Decision

`next_status: CANDIDATE`

Reason: `probelabs/probe#568` is fresh, public, relevant to coding-assistant code-search reliability, has concrete output/stack trace, and has no exact open PR. It is not `PR-READY` because current-main/release repro is missing and the suspected root cause is not yet proven.

No runner repro was run in this cycle because this was repo-scope scouting. `corp-opensource-runner` should be used for the next step; CT `216` was not used because this is not Hermes-specific.

## Repro Plan

1. In `corp-opensource-runner`, pin the reported release path: `@probelabs/probe@0.6.0-rc319` and/or current `main`.
2. Create a minimal source file with functions/classes.
3. Run both public paths:
   - `npx -y @probelabs/probe@0.6.0-rc319 symbols <file> --format json`
   - Node SDK `symbols({ files: [...] })`
4. Capture stdout, stderr, exit code and first 200 bytes parsed by the wrapper.
5. If fail-before is confirmed, repeat duplicate search over `#568`, `symbols`, `stdout`, `json`, `#205`, `#109`; then decide between `COMMENT-FIRST` with evidence or `PR-READY` with targeted regression test.

## Probe #568 follow-up, 23:07 UTC

Bounded refresh по `probelabs/probe#568`: 6 read-only subagents, parent live gates через `gh`, затем 3-role critique. Upstream PR/comment не создавались.

Tracker update: [#80 comment](https://github.com/serejaris/corp-opensource/issues/80#issuecomment-4559411989).

### Parent live gates

- Issue: `probelabs/probe#568` open, unassigned, labels `bug` + `external`, updated `2026-05-27T15:17:13Z`.
- Upstream response: `probelabs` / Visor подтвердил общий bug-shape, но прямо запросил exact command/API call, env/verbose flags и consistency. Комментарий без этих данных будет шумом.
- Duplicate PR gate: exact open PR под `symbols` stdout/JSON pollution не найден; live search вернул unrelated open `#264` и merged historical/context PRs. `#205`, `#546`, `#560` считать precedent/context, не coverage.
- Repo gate: `probelabs/probe` остаётся small viable code-search lane: Apache-2.0 root license, default branch `main`, `616` stars, `13` open issues, `2` open PRs, pushed `2026-05-21T17:02:13Z`.
- Runner gate: repro не запускался. `ssh corp-server ...` сейчас failed with `Could not resolve hostname corp-server`, dedicated `corp-opensource-runner` не подтверждён, CT `216` не применим because this is not Hermes-specific.

### 6-subagent synthesis

| Роль | Вывод |
|---|---|
| Factology/root cause | Симптом конкретный и likely связан с stdout/stderr pollution, но exact path не доказан; `symbols.rs::handle_symbols` может быть JSON-clean, а `Pattern:`/`Path:` likely из search/debug path. |
| Duplicate/race | Exact open duplicate PR не найден; historical merged JSON/stdout PRs are precedent only. |
| Process/community | `COMMENT-FIRST` без runner-backed exact command/API/env не allowed, потому что upstream уже запросил именно эти details. |
| Repro/runner | Нужен runner fail-before на `@probelabs/probe@0.6.0-rc319` и current `main`, CLI + Node SDK, stdout/stderr/exit/parse capture. |
| Patch/test surface | Patch likely small only after repro; test card must prove JSON stdout clean for `symbols --format json` and debug/progress routed to stderr without breaking text output. |
| Final synthesis | `CANDIDATE`, not `COMMENT-FIRST` and not `PR-READY`, until exact repro evidence exists. |

### 3-role critique

- Factology/duplicates: approve revised. Use `CANDIDATE`; no exact open PR found; historical `#205/#546/#560` are precedent/context; runner gap must be recorded.
- Process: approve. 6-subagent synthesis and parent live gates complete; no upstream action; CT `216` must not be used for Probe.
- Actionability: approve. Next step is runner-backed fail-before, then repeat duplicate search and decide between `COMMENT-FIRST` with evidence vs `PR-READY`.

Decision: `next_status: CANDIDATE`.

Unblock event: in `corp-opensource-runner`, capture exact command/API/env for `0.6.0-rc319` and current `main`, raw stdout/stderr, exit code, JSON parse result, and repeat duplicate PR search immediately before any upstream comment or PR.

## Probe #568 correction heartbeat, 2026-05-28 01:14 UTC

Six-lane readiness refresh plus 3-role critique kept `probelabs/probe#568` as `CANDIDATE`, with one wording correction: the upstream bot/maintainer response is a positive bug-shape confirmation, not only a request for missing details. It also asks for exact CLI/API path, environment/flags, and consistency details, so upstream reply remains blocked until we can answer with evidence.

Live gate summary:

- Issue remains open, unassigned, labeled `bug` + `external`.
- Exact open PR for `symbols` JSON stdout pollution was not found in this pass.
- Source-path critique still says the exact pollution source is unproven: `symbols.rs::handle_symbols` appears JSON-clean, while `Pattern:` / `Path:` output may come from the search/debug path.
- Runner/tooling gate still blocks: `corp-server` unresolved, no dedicated runner available, and local env lacks `rustc/cargo`.

Decision remains `next_status: CANDIDATE`; upstream action count `0`.

Fallback runner target after `mini-swe#826`: reproduce `@probelabs/probe@0.6.0-rc319` and current `main` through both CLI and Node SDK, capturing raw stdout, stderr, exit code, JSON parse result, command/API call, env and flags.

## Probe #568 backlog refresh, 2026-05-28 02:19 UTC

Fresh candidate backlog refresh: [watch note](candidate-backlog-refresh-2026-05-28-0219.md).

Six read-only roles plus 3-role critique promoted `probelabs/probe#568` from fallback runner target to primary next runner target for the candidate backlog. Upstream action count remains `0`.

Why primary now:

- issue is still open, unassigned, labeled `bug` + `external`;
- upstream `probelabs` / Visor response confirms legitimate bug shape and asks for exact CLI/API/env evidence;
- no exact open PR cover was found; open PRs `#561/#264` are unrelated;
- runner feasibility is better than `serena#1519`, `BrowserOS#1005`, `emdash#1875`, `ag-ui#1635`, and `mastra#17118` because Probe needs a Rust/CLI JSON capture, not Docker/Bun/Electron/browser.

Critical caveat: source review still does not prove the root cause. `src/extract/symbols.rs::handle_symbols` appears JSON-clean, while suspicious `Pattern:` / `Path:` stdout output is in `src/main.rs::handle_search`. The runner job must first prove the exact failing CLI/API path before any upstream comment or PR.

Updated runner card:

1. Capture `v0.6.0-rc319` and current `main`.
2. Run CLI and Node SDK paths for `symbols --format json`.
3. Record command/API call, fixture, env/flags, stdout, stderr, exit code, JSON parse result and Node wrapper parse result.
4. Repeat duplicate search for `#568`, `symbols`, `stdout`, `json`, `#205`, `#109`, `#546`, and `#560`.
5. Only then choose `COMMENT-FIRST` with evidence or `PR-READY` with a focused regression test.
