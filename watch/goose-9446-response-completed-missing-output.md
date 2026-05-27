# aaif-goose/goose #9446: response.completed missing output

Date: 2026-05-27

## Upstream

- Issue: https://github.com/aaif-goose/goose/issues/9446
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
- `cargo fmt --check`: passed
- Existing warning observed during tests: `method clear_env is never used` in `crates/goose/tests/providers.rs`; unrelated to this patch.

## Next gate

Before upstream PR:

1. Re-run duplicate search and issue timeline.
2. Re-read PR template if present and current repo rules.
3. Decide whether targeted test + `cargo fmt --check` is enough for draft, or run broader `cargo test -p goose`.
4. Use signed commit (`git commit -s` required by repo AGENTS).
