# Claude Code #63019 Agent Teams permission TUI corruption

Дата: 2026-05-28

Upstream: https://github.com/anthropics/claude-code/issues/63019

Trackers:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561433388
- High-churn terminal/runner: https://github.com/serejaris/corp-opensource/issues/74#issuecomment-4561434267

Статус: `LEAD`

Upstream actions: `0`

Runner actions: `0`

## Коротко

`anthropics/claude-code#63019` reports terminal output corruption when a permission prompt appears while the user is navigating the expanded Agent Teams selection UI. The report is fresh, has screenshots, and includes a concrete repro path with `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` on Claude Code `2.1.153`.

This is relevant to Paperclip-like / AI-native workflows because it sits at the boundary of agent orchestration, permissions, and terminal UI state. It is not a runner-first candidate yet: reproduction is macOS/TUI/manual, the repo appears issue-first for external contributors, and no independent repro or root-cause path is available from the current environment.

## Parent live gates

- Issue: open, unassigned, labels `bug`, `has repro`, `platform:macos`, `area:tui`, `area:agents`, `area:permissions`; comments absent at gate time.
- Repo: `anthropics/claude-code`, ~127k stars, active `main`, GitHub license detection absent.
- Contribution/process: root listing shows `LICENSE.md`, `README.md`, `SECURITY.md`, but no `CONTRIBUTING` file in the top-level contents check. Treat as issue-first / maintainer-directed until proven otherwise.
- Repro surface: macOS terminal, Claude Code Agent Teams enabled, expanded agent selection UI open, permission prompt appears during selection navigation.
- Environment nuance: issue form says iTerm2, but the report states screenshots were captured in Ghostty and the issue was also reproduced in iTerm2 and another terminal emulator. Record as multi-terminal on macOS, not iTerm2-only.
- Duplicate/PR search: no exact open PR cover found for `#63019`, Agent Teams selection UI, permission prompt, and terminal corruption.
- Runner: `corp-opensource-runner` remains unavailable via tracker `#10`; Linux runner would not cover the macOS TUI surface anyway.

## Duplicate and prior-art notes

Nearby but not exact:

- `#49865`: closed Agent Teams / permission crash around `getAppState`; related state/permission renderer area, but different symptom and older fixed path.
- `#8618`: broad terminal rendering corruption / scrolling instability on older Claude Code `2.0.1`; too broad to cover the Agent Teams permission prompt selection-menu trigger.
- Other adjacent classes mentioned by critique: permission prompt disappearance or Agent Teams renderer crashes around prompt stacking, but not the same garbled text during selection navigation.

Duplicate risk is medium because the area is noisy, but no exact cover was found.

## Repro plan

Minimum evidence before promotion:

1. macOS terminal with Claude Code `2.1.153` or current latest.
2. Start with `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`.
3. Trigger an Agent Teams workflow that spawns multiple agents.
4. Open the expanded agent list with Shift+Down and navigate the selection.
5. Trigger or wait for a permission prompt while selection focus remains in the Agent Teams UI.
6. Capture terminal buffer before/after prompt, terminal app, shell, `TERM`, tmux/no-tmux, and whether dismissing the prompt restores the display.

Ideal later test is a TUI snapshot/buffer regression around simultaneous selection-menu navigation and permission prompt overlay. Without an upstream TUI harness, this remains manual-repro first.

## 6-role synthesis

Fresh discovery after `2026-05-28T06:33:00Z` found `#63019` as the only material new lead. Active PR dashboard and existing runner backlog had no material transition. Core candidate order remains unchanged: `probelabs/probe#568`, `google-gemini/gemini-cli#27503`, `vercel/ai#15652`, `langchain4j/langchain4j#5313`; `openai/codex#24873` is a strong but WSL/Desktop-specific candidate behind infra evidence.

## Critique

- Factology/duplicates: pass. The issue is well scoped and no exact PR/duplicate cover was found; adjacent issues are related but not exact.
- Process gates: do not comment or PR now. A useful upstream comment would need new evidence, not a restatement of the reporter's screenshots/repro.
- Actionability: keep below `CANDIDATE` until independent macOS/TUI repro or a narrowed terminal-buffer failure mode exists.

## Decision

`next_status: LEAD`

Track as a fresh Claude Code Agent Teams/TUI permission lead. No upstream comment/PR. Recheck for maintainer labels/assignee/linked PR or a reproducible macOS/TUI evidence path.
