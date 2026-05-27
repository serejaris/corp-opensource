# AI-native frameworks scouting cycle 6

Дата: 2026-05-27

Метод: active queue cleanup + duplicate-confirmation pass.

## Вывод

`corp-opensource#21` больше не является candidate-bug lane. Upstream `OpenHands#14476` уже covered by active PR `OpenHands#14489`, поэтому internal issue закрыт.

Internal issue closed: https://github.com/serejaris/corp-opensource/issues/21

## OpenHands #14476 duplicate coverage

Upstream issue: https://github.com/OpenHands/OpenHands/issues/14476

Existing PR: https://github.com/OpenHands/OpenHands/pull/14489

Live status on 2026-05-27:

- PR state: open.
- Mergeable: mergeable.
- Review decision: review required.
- Checks: Python tests, enterprise unit tests, lint, package/version checks green; Docker jobs skipped by workflow conditions.
- Files:
  - `openhands/app_server/app_conversation/sql_app_conversation_info_service.py`
  - `tests/unit/app_server/test_sql_app_conversation_info_service.py`
- Behavior: recover duplicate-key race by rolling back failed insert and updating existing row.
- Tests: regression for concurrent insert path.

Decision: `CLOSE / duplicate-covered`.

No new PR. Reopen only if #14489 is closed without merge and #14476 remains reproducible, or if a distinct SaaS metadata org-mismatch path remains after #14489.

## Lesson

Internal candidate issues can go stale quickly. When a later duplicate sweep finds a sufficient active PR with tests, close the internal candidate instead of keeping it in the active queue. Being “interesting” is not enough; the queue should represent work we can still act on.
