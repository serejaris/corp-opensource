# AI-native Frameworks Scouting 2026-05-27 Cycle 13

## Фокус

Повторный scouting после открытия `opencode#29565`: найти следующий upstream target, но не открывать PR без fresh duplicate gate, regression card и repo-ready правил.

GitHub Search API во время цикла упёрся в rate limit, поэтому широкий search был ограничен. Субагенты использовали live issue/PR views, issue timelines, known tracker candidates и raw `main` code/test surfaces where needed.

## Субагенты

| Роль | Итог |
|---|---|
| Repo-fit | Лучшие repo-fit: `pydantic-ai`, `opencode`, `cline`, `OpenHands/software-agent-sdk`, `trycua/cua`, `E2B`; но активные PR уже ждут review/CLA/eval, поэтому новый PR не начинать. |
| Bug-signal | Свежие сигналы: `pydantic-ai#5679`, `pydantic-ai#5671`, `browser-use#4846`, `agno#8113`, `vercel/ai#13013`, MCP TS/Python issues. |
| Repro-path | Repro-friendly lanes: `pydantic-ai#5387/#5606`, MCP Python `#2687`, Playwright MCP `#1635`, goose `#9136`; все требуют assignment/comment/maintainer direction before PR. |
| Patchability | Самый маленький patch surface: MCP Python `#2687`; затем Codex `#24725`, pydantic-ai `#5606`, goose `#9136`. Но patchability не отменяет repo gates. |
| Duplicate-race | Безопасный новый PR-кандидат не всплыл. Сильные browser-use/agno/MCP TS/pydantic lanes уже покрыты open PR или требуют missing-regression comment. |
| PR-readiness | PR-ready repos: `opencode`, `cline`, `CopilotKit`, MCP TS for tiny scoped bugs. Hard no-cold-PR: `pydantic-ai`, `deepagents`, MCP Python, goose current issues, Playwright MCP current issue. |

## Candidate Decisions

| Candidate | Decision | Why |
|---|---|---|
| `modelcontextprotocol/python-sdk#2687` | `COMMENT-FIRST / WAIT` | Best small patch and regression surface, but repo lane is maintainer-confirmation/assignment before PR. Existing comment already offered the fix. |
| `pydantic/pydantic-ai#5606` | `COMMENT-FIRST / SPLIT IF APPROVED` | Patchable if limited to profile/model-name mapping, but pydantic non-trivial PRs need maintainer agreement. |
| `pydantic/pydantic-ai#5387` | `WATCH / NOT CLEAN` | Previously attractive offline repro, but current `main` has `run_on_errors` handling/tests; not a clean fresh PR lane. |
| `browser-use#4846` | `WATCH / DUPLICATE-COVERED` | Open PRs `#4847` and `#4881` cover the MCP CDP lifecycle area. Review only if comparing lifecycle depth. |
| `browser-use#4801` | `WATCH / COMPETING PRS` | Open PRs `#4835` and `#4880`; maintainer should pick approach. |
| `agno#8113` | `WATCH / NEEDS REPRO` | Interesting AgentOS SSE/history signal, but no regression card yet and `agno` has recent duplicate auto-close risk. |
| `vercel/ai#13013` | `WATCH / NEEDS REPRO` | Strong tool-loop pain, but needs repo-specific repro/readiness pass before PR. |
| `openai/codex#24725` | `WATCH / ACCESS-SENSITIVE` | Tiny patch surface, but external PR path is constrained; comment-first only. |
| `goose#9136` | `COMMENT-FIRST / WAIT` | Patchable but assignment-gated; existing offer posted. |
| `Playwright MCP#1635` | `COMMENT-FIRST / WAIT` | Needs maintainer target-repo confirmation; code may live in Playwright monorepo. |
| `E2B#1354` | `ACTION / CLA` | Current PR is ready but blocked by CLA. User must sign CLA, then comment `@cla-bot check`. |

## Decision

No new upstream PR should be opened from this cycle.

The correct next actions are:

1. Sign E2B CLA and trigger `@cla-bot check` on `E2B#1354`.
2. Continue watching active PRs: `opencode#29565/#29530`, `pydantic-ai#5678/#5680/#5681`, `cline#11087`, `OpenHands SDK#3394`, `CopilotKit#5035`.
3. For MCP Python `#2687`, pydantic-ai `#5606`, goose `#9136`, Playwright MCP `#1635`: wait for maintainer confirmation before PR.
4. Next scouting cycle should focus only on PR-ready repos with no active owned PR queue: `MCP TypeScript`, `cline` only after `#11087` moves, `opencode` only after current PR review, or a fresh repo not already saturated by competing PRs.

## Урок по тестам

Тестовый урок из Hermes/Codex и текущего цикла:

- сначала duplicate/assignment gate;
- потом regression card: `контракт`, `test file`, `command`, `pre-fix fail`;
- затем pre-fix failing test или reverted-fix failing test;
- только после этого patch;
- если patch легко пишется, но repo требует maintainer assignment, результат остаётся `COMMENT-FIRST / WAIT`, а не PR.

`Patchability` не является разрешением на PR. Это только один из шести сигналов.
