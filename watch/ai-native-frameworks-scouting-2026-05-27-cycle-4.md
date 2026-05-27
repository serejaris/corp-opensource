# AI-native frameworks scouting cycle 4

Дата: 2026-05-27

Метод: 6 субагентов — repo-fit, bug-signal, repro-path, patchability, duplicate-race, PR-readiness.

## Вывод

Следующий реальный кандидат: [aaif-goose/goose#9136](https://github.com/aaif-goose/goose/issues/9136).

PR пока не открывать. Правильный шаг — comment-first и ждать maintainer assignment/green light, потому maintainer уже назвал баг реальным, но писал “before we pick this up”.

Upstream comment posted: https://github.com/aaif-goose/goose/issues/9136#issuecomment-4552499743

Internal tracker: https://github.com/serejaris/corp-opensource/issues/28

## Candidate status

| Candidate | Status | Decision |
|---|---|---|
| [opencode#29294](https://github.com/anomalyco/opencode/issues/29294) | Covered by open PR [opencode#29108](https://github.com/anomalyco/opencode/pull/29108) | No-go / watch only |
| [goose#9136](https://github.com/aaif-goose/goose/issues/9136) | Open, unassigned, snooze date expired, maintainer confirmed real bug | Comment-first; wait assignment before PR |

## Goose #9136 triage

Problem:

- One model response can contain many identical tool requests.
- Reporter saw >100 calls, 32 GB memory spike, system freeze, goose killed.
- Current `main` still creates `RepetitionInspector::new(None)`.
- Current `inspect()` creates a fresh temp inspector per request, so state does not accumulate across requests in the same batch.

Maintainer signal:

- “Triaged: this is a real bug.”
- “Fix plan (small, one-shot):”
- Set default `max_repetitions`, e.g. 3.
- Accumulate state across tool requests within the same batch.
- Expose `GOOSE_MAX_TOOL_REPETITIONS`.
- Make `max_repetitions` a `u32` instead of `Option<u32>`.

Duplicate scan:

- No active PR found for this exact `RepetitionInspector` / same-batch repetition bug.
- [#2527](https://github.com/aaif-goose/goose/pull/2527) is merged predecessor, not current fix.
- [#8994](https://github.com/aaif-goose/goose/pull/8994) is closed/unmerged and targets repeated tool errors, not same-batch identical requests.
- [#6497](https://github.com/aaif-goose/goose/pull/6497) is closed/unmerged.
- [#7758](https://github.com/aaif-goose/goose/pull/7758) is adjacent MOIM/Gemini loop ordering, not this fix.

## Likely patch surface

- `crates/goose/src/tool_monitor.rs`
  - `RepetitionInspector`
  - `RepetitionInspector::new`
  - `RepetitionInspector::check_tool_call`
  - `ToolInspector for RepetitionInspector::inspect`
- `crates/goose/src/agents/agent.rs`
  - currently adds `RepetitionInspector::new(None)`
- `crates/goose-cli/src/session/builder.rs`
  - already has `SessionBuilderConfig.max_tool_repetitions`
- `crates/goose-cli/src/cli.rs`
  - already has `--max-tool-repetitions`
- docs if env var is added:
  - `documentation/docs/guides/environment-variables.md`
  - `documentation/docs/guides/config-files.md`

## Regression shape

Existing test only covers `check_tool_call()` helper. That is insufficient.

The first regression should cover the public inspector path:

```bash
cargo test -p goose --test repetition_inspector_tests test_repetition_inspector_inspect_denies_repeated_tool_requests_in_one_batch -- --exact
```

Expected pre-fix failure:

```text
assertion `left == right` failed
  left: 0
 right: 1
```

Reason: `inspect()` checks each request against a cloned stale state, so 3 identical `ToolRequest`s in one batch do not trigger a deny.

## Repo process

- Fork PRs accepted.
- Base branch: `main`.
- Branch suggestion: `fix/9136-tool-repetition-limit`.
- Title suggestion: `fix(agent): limit repeated tool calls`.
- Commit requires DCO signoff: `git commit -s`.
- PR template sections: Summary, Testing, Related Issues.

## Next

1. Wait for maintainer response/assignment on upstream comment.
2. If green-lit, run fresh duplicate scan again.
3. Write failing `inspect()` regression before implementation.
4. Implement narrow fix and run targeted Rust tests through Hermit:
   - `source bin/activate-hermit`
   - `cargo test -p goose repetition_inspector`
   - `cargo fmt`
   - `cargo clippy --workspace --all-targets --exclude v8 -- -D warnings` if feasible.

## Local harness check

On 2026-05-27, local checkout `/Users/ris/Documents/GitHub/goose` was created and Hermit bootstrapped Rust:

```bash
source bin/activate-hermit
cargo --version
rustc --version
```

Result:

- `cargo 1.92.0`
- `rustc 1.92.0`

Existing focused test command:

```bash
cargo test -p goose repetition_inspector
```

Result: passed, 1 existing test ran in `tests/repetition_inspector_tests.rs`.

This only proves the current helper-level harness is runnable. It does not prove #9136 is fixed, because the missing regression is for `ToolInspector::inspect()` over a batch of repeated `ToolRequest`s.
