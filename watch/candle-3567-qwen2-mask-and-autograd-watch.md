# Candle #3567 Qwen2 mask and autograd watch

Дата: 2026-05-28

Upstream:

- https://github.com/huggingface/candle/issues/3567
- https://github.com/huggingface/candle/issues/3568
- https://github.com/huggingface/candle/issues/3569
- https://github.com/huggingface/candle/pull/3526

Trackers:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561668867

Статус: `CANDIDATE`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Fresh discovery after `2026-05-28T07:08:00Z` found a `huggingface/candle` correctness/autograd cluster. Parent decision: select `#3567` as the candidate, and keep `#3568/#3569` as watch/coordination lanes.

- `#3567`: Qwen2 `prepare_attention_mask` appears to create only a padding mask when `attn_mask` is provided, without a causal triangle. This can silently make batched eval/training bidirectional and produce wrong metrics.
- `#3568`: `rotary_emb::rope` / variants use `apply_op*_no_bwd`, severing autograd. Related to the same pattern as `#2168` / PR `#3526`, but not covered by it.
- `#3569`: `ops::softmax_last_dim` uses `apply_op1_no_bwd`, also severing autograd. Related to `#3568` and `#3526`, but not covered by them.

## Parent live gates

- Repo: `huggingface/candle`, ~20k stars, active `main`, top-level `Cargo.toml`, `.cargo`, `LICENSE-APACHE`, `LICENSE-MIT`, `README.md`; GitHub API license detection absent.
- `#3567`: open, unassigned, no labels, no comments at gate time.
- `#3568`: open, unassigned, no labels, no comments at gate time.
- `#3569`: open, unassigned, no labels, no comments at gate time.
- Related PR `#3526`: open, non-draft, `MERGEABLE`; fixes RmsNorm backward pass for `#2168` and is a precedent for the `apply_op*_no_bwd` family, not an exact cover for `rope` or `softmax_last_dim`.
- Duplicate/PR search: no exact PR cover found for `#3567`, Qwen2 causal mask, `#3568` rope backward, or `#3569` softmax backward.
- Runner: `corp-opensource-runner` remains unavailable via tracker `#10`; no Candle repro/test was run.

## Repro plan

For `#3567`:

1. Use CPU-only Candle current `main`.
2. Exercise Qwen2 attention-mask construction with `attn_mask` present.
3. Assert self/lower-triangle entries are allowed and upper-triangle future-token positions are `-inf` or otherwise blocked.
4. Include padding + causal combination, not only the all-ones mask case.
5. If direct testing of `prepare_attention_mask` is awkward because it is `pub(crate)`, use the smallest upstream-friendly model/helper test that proves the same mask semantics.

For `#3568/#3569`:

1. CPU-only autograd unit tests.
2. For `softmax_last_dim`, use a non-constant loss such as `(out * out).sum_all()` and assert upstream grads exist; compare with `softmax(t, D::Minus1)`.
3. For `rope`, compare fast `rope` variants against differentiable `_slow` variants.
4. Do not PR cold until maintainer preference is clear: docs-only warning vs real backward implementation.

## 6-role synthesis

`#3567` is the cleanest candidate because it is isolated, CPU-testable, and about silent correctness in Qwen2 batched eval/training. `#3568/#3569` are valid, but they touch shared autograd/custom-op design and are coordination-sensitive because `#3526` is already open for the same `apply_op*_no_bwd` family.

Existing runner backlog remains Probe, Gemini, Vercel `#15652`, LangChain4j, then Nanobot/Hermes fallback candidates. Candle `#3567` is a good future Rust/CPU candidate if runner capacity opens, but it does not become `PR-READY` without current-main repro.

## Critique

- Factology/duplicates: pass for `#3567` as independent `CANDIDATE`; `#3568/#3569` are not exact-covered by `#3526`, but they carry coordination risk.
- Process gates: no upstream comment/PR. Issues already include repro links and suggested fixes; comment without runner confirmation would add little.
- Actionability: `#3567` has a focused CPU test path; `#3568/#3569` need maintainer direction or stronger test/implementation framing.

## Decision

`next_status: CANDIDATE`

Track `#3567` as the selected Candle candidate. Keep `#3568/#3569` as `WATCH / coordination-first` until `#3526` settles or maintainers indicate preferred handling.
