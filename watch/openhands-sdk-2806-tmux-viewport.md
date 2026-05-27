# OpenHands SDK #2806 - tmux viewport CPU

## Upstream

- Issue: https://github.com/OpenHands/software-agent-sdk/issues/2806
- PR: https://github.com/OpenHands/software-agent-sdk/pull/3394
- Fork branch: https://github.com/serejaris/software-agent-sdk/tree/fix/2806-reduce-tmux-session-size
- Internal issue: https://github.com/serejaris/corp-opensource/issues/22

## Суть

`openhands-tools` создавал tmux sessions с viewport `1000x1000`. Reporter upstream показал, что detached `tmux: server` тратит CPU в kernel mode на обработку PTY output по огромному virtual terminal grid.

## Проверка duplicate gate

Проверено 2026-05-27:

- upstream issue `#2806` открыт, `help wanted`;
- maintainer ответил: `feel free to open a PR`;
- поиск открытых и закрытых PR по `2806`, `tmux 1000`, `terminal size`, `x=1000 y=1000`, `burns CPU` не нашёл competing PR;
- старый OpenHands main PR по этой теме был закрыт как wrong repo; canonical owner теперь `OpenHands/software-agent-sdk`.

## Изменение

PR `#3394`:

- снижает `TMUX_SESSION_WIDTH/HEIGHT` с `1000x1000` до `256x200`;
- не меняет `HISTORY_LIMIT = 10_000`;
- добавляет regression test против возврата oversized viewport.

## Локальная валидация

Pre-fix regression:

```text
uv run pytest tests/tools/terminal/test_tmux_pane_pool.py::test_tmux_session_viewport_is_smaller_than_scrollback -q
FAILED: assert 1000 <= 256
```

Post-fix:

```text
uv run pytest tests/tools/terminal/test_tmux_pane_pool.py::test_tmux_session_viewport_is_smaller_than_scrollback -q
1 passed

uv run pytest tests/tools/terminal/test_tmux_pane_pool.py tests/tools/terminal/test_observation_truncation.py tests/tools/terminal/test_ps1_corruption.py tests/tools/terminal/test_heredoc_chunked_send.py -q
44 passed

uv run pytest tests/tools/terminal -q
285 passed, 9 skipped

uv run pre-commit run --files openhands-tools/openhands/tools/terminal/constants.py tests/tools/terminal/test_tmux_pane_pool.py
passed
```

Manual tmux smoke:

```text
tmux_size=256x200
session_history-limit 10000
```

## Текущее состояние

2026-05-27:

- PR open, mergeable;
- review required;
- maintainer suggestion from VascoSch92 applied in `efdff15b`: pin exact constants in regression test;
- validation after suggestion:
  - `uv run pytest tests/tools/terminal/test_tmux_pane_pool.py::test_tmux_session_viewport_is_smaller_than_scrollback -q` -> 1 passed;
  - `uv run pytest tests/tools/terminal/test_tmux_pane_pool.py tests/tools/terminal/test_observation_truncation.py tests/tools/terminal/test_ps1_corruption.py tests/tools/terminal/test_heredoc_chunked_send.py -q` -> 44 passed;
  - `uv run pre-commit run --files openhands-tools/openhands/tools/terminal/constants.py tests/tools/terminal/test_tmux_pane_pool.py` -> passed;
- replied on review thread with validation;
- status checks list became empty after follow-up push; follow-up: re-check checks/reviews and avoid pushing unless asked.

## Follow-up после OpenHands review

2026-05-27:

- human maintainer `VascoSch92` написал, что change looks fine, но хочет прогнать eval;
- `run-eval-50` был triggered;
- OpenHands bot оставил `CHANGES_REQUESTED` по тесту:
  - exact-value assertions были слишком хрупкими для test name;
  - `HISTORY_LIMIT == 10_000` был unrelated assertion внутри viewport test;
  - нужно явно зафиксировать wrap trade-off в комментарии;
- follow-up commit `3c8eae78` pushed:
  - test переименован в `test_tmux_session_viewport_is_bounded`;
  - assertions стали `TMUX_SESSION_WIDTH <= 256`, `TMUX_SESSION_HEIGHT <= 200`;
  - unrelated `HISTORY_LIMIT` assertion удалён;
  - comment добавлен: output шире `TMUX_SESSION_WIDTH` будет wrap, это принятый tradeoff против oversized `1000x1000` grid;
- reviewer suggestion `TMUX_SESSION_WIDTH * TMUX_SESSION_HEIGHT < HISTORY_LIMIT` не применён, потому что при целевых значениях он false: `256 * 200 = 51200`, `HISTORY_LIMIT = 10000`;
- validation после rebase на свежую fork branch:
  - `uv run pytest tests/tools/terminal/test_tmux_pane_pool.py::test_tmux_session_viewport_is_bounded -q` -> 1 passed;
  - `uv run pre-commit run --files openhands-tools/openhands/tools/terminal/constants.py tests/tools/terminal/test_tmux_pane_pool.py` -> passed;
- PR comment: https://github.com/OpenHands/software-agent-sdk/pull/3394#issuecomment-4553490302.

Post-push check:

- PR remains open and mergeable;
- GitHub still reports old `CHANGES_REQUESTED` from the previous OpenHands bot review;
- checks list is empty immediately after follow-up push, so there is no new CI/eval signal yet.

Workflow run check:

- `gh run list --repo OpenHands/software-agent-sdk --branch fix/2806-reduce-tmux-session-size --limit 10` shows runs for commit `3c8eae78`;
- all visible runs are `completed` with `conclusion=action_required`;
- this means fork workflows are waiting for maintainer approval, not failing due to code.

Следующее действие: дождаться новых CI/eval результатов и maintainer response; не пушить дальше без нового actionable review.
