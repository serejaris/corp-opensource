# aaif-goose/goose #9446: response.completed missing output

Date: 2026-05-27

## Upstream

- Issue: https://github.com/aaif-goose/goose/issues/9446
- PR: https://github.com/aaif-goose/goose/pull/9447
- Internal tracker: https://github.com/serejaris/corp-opensource/issues/49
- State checked on 2026-05-27: open, no assignee, no comments, no labels.
- Duplicate scan checked on 2026-05-27: no PR found for `#9446`, `response.completed output`, `openai_responses`, or `chatgpt_codex` missing output.

## Local patch prep

- Local checkout: `/Users/ris/Documents/GitHub/goose`
- Branch: `codex/9446-openai-response-completed-default-output`
- Local commit: `f4d5dd6b2 fix(providers): tolerate missing Responses output`
- Patch surface: `crates/goose/src/providers/formats/openai_responses.rs`
- Change: add `#[serde(default)]` to `ResponseMetadata.output`.
- Test: `test_responses_completed_accepts_missing_output`

## Regression card

- Contract: a valid terminal `response.completed` event may omit `response.output`; Goose should not abort if text has already streamed through prior events.
- Test command:

```bash
source bin/activate-hermit
cargo test -p goose test_responses_completed -- --nocapture
```

- Pre-fix failure:

```text
Failed to parse Responses stream event: missing field `output`
```

- Post-fix result:

```text
test providers::formats::openai_responses::tests::test_responses_completed_accepts_missing_output ... ok
```

## Verification

```bash
source bin/activate-hermit
cargo test -p goose test_responses_completed_accepts_missing_output -- --nocapture
cargo fmt --check
```

Results:

- targeted regression: passed
- `cargo test -p goose providers::formats::openai_responses::tests -- --nocapture`: `31 passed`
- `cargo fmt --check`: passed
- `cargo clippy -p goose --lib -- -D warnings`: passed
- Existing warning observed during tests: `method clear_env is never used` in `crates/goose/tests/providers.rs`; unrelated to this patch.

## Next gate

PR opened after final duplicate and repo-rule gate:

- PR URL: https://github.com/aaif-goose/goose/pull/9447
- PR state after creation: open, ready, mergeable, review required.
- Initial checks in tracked state: `check-quarantined` and `machete` passed; `changes` failed because GitHub API returned `diff temporarily unavailable due to heavy server load`; several downstream Rust/release/live-provider jobs were skipped because they depend on `changes`.
- Attempted `gh run rerun 26510596648 --repo aaif-goose/goose --failed`, but GitHub CLI reported the workflow cannot be rerun by this actor.
- PR comment with CI note posted: https://github.com/aaif-goose/goose/pull/9447#issuecomment-4554433919

Next action: watch CI/review; if the transient `changes` failure persists, ask a maintainer to rerun or push a tiny follow-up only if there is a real code/review reason.
