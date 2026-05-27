# AI-native frameworks scouting: cycle 19

Date: 2026-05-27
Mode: 6-subagent scouting with local live duplicate checks.
Internal tracker: https://github.com/serejaris/corp-opensource/issues/49

## Outcome

Safe new ready PR: none.

Useful action: confirmed that the next tempting candidates are duplicate-covered, assignment-gated, or need maintainer/repro confirmation. Keep current active PRs in follow-up mode.

## Active PR follow-up

| Upstream | Current state | Decision |
|---|---|---|
| `aaif-goose/goose#9447` | Open, ready, mergeable, review required. `check-quarantined` and `machete` passed. `changes` failed before path filters because GitHub returned `diff temporarily unavailable due to heavy server load`; dependent CI jobs skipped. | Wait maintainer/rerun/review; no code action. |
| `OpenHands/software-agent-sdk#3394` | Open, mergeable. Human maintainer said it looks fine and triggered eval. Review threads are resolved after follow-up `3c8eae78`, but PR still shows old `CHANGES_REQUESTED`. Fork workflows on `3c8eae78` are `action_required`. | Wait maintainer eval/workflow approval; no further push without new actionable review. |

## Six-subagent synthesis

| Target | Finding | Decision |
|---|---|---|
| `modelcontextprotocol/typescript-sdk#1582` | Open issue is `ready for work`, but direct PR `#1657` is open and covers scope accumulation across 401/403; older `#1618` was closed. `#1657` is dirty/changes-requested but has the right contract/tests. | Watch/review only; do not open duplicate PR. |
| `trycua/cua#1724` | No linked PR, but repro requires installed `CuaDriver.app`; likely Swift direct-stdio AppKit path while upstream is moving Rust/embedded. | Comment-first only after local repro or maintainer confirmation that Swift direct stdio remains supported. |
| `trycua/cua#1722` | Exact PR `#1723` was opened by the issue author and closes the issue. | Watch `#1723`; no duplicate. |
| `anomalyco/opencode#29498/#29518/#29534` | No exact open PR for these three, but all are either assigned, Windows/encoding repro-heavy, or adjacent to active opencode PRs `#29530`/`#29565`. `#29518` also overlaps tool-input repair work. | Watch/comment-first; no third simultaneous opencode PR. |
| `OpenHands/software-agent-sdk#3317` | No open exact PR now, but closed exact PR `#3319` exists and our `#3394` is still waiting eval in the same repo. | Watch; ask maintainer before any new PR. |
| Fresh candidate scout | Timed out in this pass. | Repeat in next cycle if current PR queue is still blocked. |

## Local duplicate checks

| Check | Result |
|---|---|
| `gh search prs --repo modelcontextprotocol/typescript-sdk "1582 OR insufficient_scope OR upscoping OR _scope OR scope overwrite"` | No simple search result, but issue timeline links `#1618` and `#1657`; direct PR `#1657` is open. |
| `gh api repos/modelcontextprotocol/typescript-sdk/issues/1582/timeline` | Cross-referenced `#1618` and `#1657`. |
| `gh search prs --repo trycua/cua "1722 OR uninstall.sh OR unexpected EOF OR matching )"` | No simple search result, but issue timeline links exact PR `#1723`. |
| `gh api repos/trycua/cua/issues/1722/timeline` | Cross-referenced `#1723` by the issue author. |
| `gh pr view OpenHands/software-agent-sdk#3394` | Review threads resolved; old `CHANGES_REQUESTED` still present; no PR checks reported. |
| `gh run list --repo OpenHands/software-agent-sdk --branch fix/2806-reduce-tmux-session-size` | Workflows for `3c8eae78` are `completed/action_required`, i.e. maintainer approval needed. |

## Next action

1. Continue watching `goose#9447` and `OpenHands/software-agent-sdk#3394`.
2. Do not open `MCP TS#1582`, `trycua#1722`, or another opencode PR.
3. If the queue stays blocked, next cycle should re-run fresh candidate scout first, then only proceed where there is no duplicate PR and a cheap pre-fix regression is possible.
