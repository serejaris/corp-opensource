# Popular AI-native repo universe cycle 33, 2026-05-28

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52).

Bounded fresh-discovery refresh over high-star / freshly-pushed AI-agent, MCP, coding-agent and browser-agent repos:

- `affaan-m/ECC`
- `farion1231/cc-switch`
- `esengine/DeepSeek-Reasonix`
- `browser-use/browser-harness`
- `OpenCoworkAI/open-cowork`
- `ref-tools/ref-tools-mcp`

The required `open-source-bug-scouting` skill is not installed in this environment, so this cycle used documented fallback: parent live GitHub gates, 6 read-only scouting roles in two waves, then 3-role critique before tracker/watch updates.

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, or rebase was made.

## Parent Live Gates

Live time: `2026-05-28 01:57-02:10 UTC`.

Runner/tooling gate:

- `corp-server` does not resolve from this environment.
- `corp-opensource-runner` is not provisioned per [#10](https://github.com/serejaris/corp-opensource/issues/10).
- Local toolchain: Node `v20.19.2`, Python `3.13.5`, no `pnpm`, no `bun`, no `rustc/cargo`, no Docker.
- Therefore no repo can move to `PR-READY` in this cycle.

Search caveat:

- The first broad repo search for `stars:>1000 pushed:>2026-05-20 (agent OR agents OR coding-agent OR ai-agent)` returned too many false positives.
- Parent narrowed to topic searches: `topic:ai-agents`, `topic:mcp`, `topic:coding-agent`, and `topic:browser-agent`.

| Repo | Live repo state | Best live lane | Decision |
|---|---|---|---|
| `affaan-m/ECC` | MIT, default `main`, `196,102` stars, pushed `2026-05-25`, latest `v1.10.0`, `13` issues / `16` PRs. | `#2049` Cursor `gateguard-fact-force` P0 is already fixed on `main` and in `v2.0.0-rc.1`; current blocker is npm/marketplace publication, not code. | `WATCH / release-followup`; anomaly/provenance-risk watch only. |
| `farion1231/cc-switch` | MIT, `82,806` stars, pushed `2026-05-27`, latest `v3.15.0`, `895` issues / `223` PRs. | DeepSeek/Kimi reasoning and Codex token-accounting lanes are crowded; `#3222/#3219` overlap `#3203`, and `#3190` is exact-covered by `#3189`. | `WATCH`; high-fit control-plane target, but duplicate-race is too high. |
| `esengine/DeepSeek-Reasonix` | MIT, `11,741` stars, pushed `2026-05-28`, latest `desktop-v0.53.0`, `250` issues / `10` PRs. | `#2051` and `#2081` are exact-covered by `#2068` and `#2089`; `#2067` looks stale/risky because TUI loop fixes already landed and root cause is disputed. | `WATCH`; re-enter only with fresh current-version repro. |
| `browser-use/browser-harness` | MIT, `13,894` stars, pushed `2026-05-20`, no latest release, `13` issues / `91` PRs. | `#382` `press_key` double-types printable characters looked actionable, but live duplicate gate found open PR `#346` as exact/super-strong cover and `#332/#297` as adjacent `press_key` covers. | `WATCH`; no candidate tracker while the covers are open. |
| `OpenCoworkAI/open-cowork` | MIT, `1,436` stars, pushed `2026-05-25`, latest `v3.3.1`, `13` issues / `27` PRs. | `#216` reporter says solved; `#220/#162` are assigned or broad and need separate platform/dependency repro. | `WATCH`; good smaller desktop/MCP watch, no current candidate. |
| `ref-tools/ref-tools-mcp` | MIT, `1,113` stars, pushed `2026-05-06`, no latest release, `3` issues / `2` PRs. | Open issues are product/support/reference asks, not patchable bug lanes. | `NO-GO` for bug hunting; low-noise reference watch only. |

## 6-Role Synthesis

- Repo fit/activity: `cc-switch`, `DeepSeek-Reasonix`, `browser-harness`, and `open-cowork` are strong Paperclip-like / AI-native watch additions. `ECC` is thematically relevant but has high provenance/anomaly risk. `ref-tools-mcp` is useful as a reference lane, not a bug target.
- Bug signal: the strongest initial lane was `browser-use/browser-harness#382`; source on `main` still sends `text` on `keyDown` and then a separate `char` event in `press_key()`.
- Duplicate/race: later process/test roles found the decisive blocker: `browser-harness#346` is open, non-draft, mergeable, changes `src/browser_harness/helpers.py`, and removes `text` from `keyDown`. `#332` and `#297` also modify `press_key` and `tests/unit/test_helpers.py`.
- Process: `browser-harness#382` is open, unassigned, labeled `in-progress`, with a member comment asking for PR. That signal is outweighed by the open exact/adjacent covers.
- Runner priority: no runner action in this cycle. If the covers close without resolving `#382`, the regression can be tested with a unit mock around `helpers.press_key("x")` plus a Chrome/CDP smoke for real insertion behavior.

## 3-Role Critique

- Factology/duplicates: demote `browser-harness#382` from apparent `CANDIDATE` to `WATCH`. `#346` is exact/super-strong cover; `#332/#297` create additional duplicate-race over the same helper path.
- Process gates: `COMMENT-FIRST` is not useful now because upstream already requested a PR and existing covers are live. `PR-READY` is impossible without runner evidence and clean duplicate gate.
- Actionability: do not create a separate internal candidate issue for `#382`. Track it as a high-signal watch lane and re-enter only after cover PRs settle.

## Decision

`next_status: WATCH`

Selected lane: none.

Upstream action count: `0`.

Runner action count: `0`.

No internal candidate issue was created.

## Re-entry Gate: `browser-harness#382`

Re-open as `CANDIDATE` only if:

1. `browser-use/browser-harness#346` closes without merge, or merges/closes but `#382` remains reproducible on current `main`.
2. `#332/#297` are also checked for overlap/merge outcome.
3. Fresh duplicate search for `#382`, `press_key`, `duplicate character insertion`, `contenteditable`, `fill_input`, and `dispatchKeyEvent` is clean.
4. Runner repro proves the fail-before:
   - unit payload contract: `press_key("x")` should not put `text` on `keyDown`; only the `char` event should carry `text="x"`;
   - browser/CDP smoke: focused `<input>` and `contenteditable` receive one character, not doubled.

Likely runner command shape:

```bash
python3 -m venv .venv
. .venv/bin/activate
python -m pip install -e . pytest
pytest tests/unit/test_helpers.py -q
```

Browser smoke needs Chromium/Chrome with remote debugging in the dedicated runner.
