# Два ответа Agno Q&A — review перед публикацией

Дата: 2026-09-27. Статус: **READY FOR OWNER REVIEW — не опубликовано**. Ничего не отправлено.

Цель — дать проверяемые полезные ответы на реальные вопросы. Принятие ответа автором и начисление Galaxy Brain не гарантируются. Публикация требует отдельного явного разрешения владельца.

## Выбор тредов

Live GraphQL проверил тела, все comments и replies. У выбранных [#7415](https://github.com/agno-agi/agno/discussions/7415) и [#7665](https://github.com/agno-agi/agno/discussions/7665) комментариев нет, accepted answer нет. Это самостоятельные технические вопросы одного проекта.

Исключены: #10383 уже подробно отвечает на content/AG-UI; #10101 имеет ответ с callable API key и offline repro; #3207 имеет два подробных ответа и storage repro; #7271 имеет ответ о dynamic tools, а различие per-run/same-run discovery недостаточно конкретизировано автором. Повторные ответы туда не готовились.

Источник: полные публичные модули `workflow.py`, `loop.py`, `condition.py`, `step.py`, `types.py`, HITL utility в тегах `v2.6.0` и `v3.0.11`. Последний release на момент проверки — `v3.0.11` от 23.09.2026. Три независимых Grok-прогона получили этот код: loop, nested review, adversarial. По transcript подтверждено чтение соответствующих функций, первоисточников и дополнительных веток. После покрытия узкого вопроса прогоны остановлены по насыщению до отдельных финальных ответов. Итоговый синтез выполнен локально; три завершённых независимых финальных аудита не заявляются. Никакие приватные файлы, секреты или данные пользователей в source packet не включались.

## 1. #7415 — original input и история всех итераций

Адрес публикации: https://github.com/agno-agi/agno/discussions/7415

Почему полезно: в треде нет версии и комментариев. Ответ точно ограничен актуальным тегом; исправляет смешение `input`, предыдущего результата и накопленного списка. Ответ описывает конкретный pattern для custom executor; snippet реализации намеренно не включён без runtime-проверки.

### Точный английский текст

Based on the **v3.0.11 source** (the question doesn't specify a version), the original input remains available as `step_input.input`.

[`Loop._update_step_input_from_outputs`](https://github.com/agno-agi/agno/blob/v3.0.11/libs/agno/agno/workflow/loop.py#L377-L416) keeps `input=step_input.input`; forwarding updates `previous_step_content` and `previous_step_outputs`. The output map is keyed by step name, so repeated executions overwrite that step's entry. It is not a history of every iteration.

For a full history accessible **inside** the next iteration, keep an explicit list in `run_context.session_state`:

1. Add a custom initialization step immediately before the loop. Set a namespaced state entry such as `state["refinement"] = {"original": deepcopy(step_input.input), "outputs": []}`.
2. Declare your iteration executor as `def iterate(step_input: StepInput, run_context: RunContext) -> StepOutput`. Read the saved original input and a snapshot of `outputs` before doing the work.
3. Append a copy of the successful result to that list, then return the normal `StepOutput`. Each subsequent iteration can read all earlier results.

The source [injects `run_context` into custom executors](https://github.com/agno-agi/agno/blob/v3.0.11/libs/agno/agno/workflow/step.py#L1011-L1023) and [shares its session-state dictionary between workflow steps](https://github.com/agno-agi/agno/blob/v3.0.11/libs/agno/agno/workflow/step.py#L1062-L1078). The official [workflow session-state guide](https://docs.agno.com/state/workflows/overview) shows this access pattern.

Reset the list in the initialization step if it should cover one workflow run only. Keep it out of a module-level variable; serialize concurrent writes to the same persisted session. If an iteration can be retried, make the append idempotent. An agent step also needs the chosen history explicitly supplied to its prompt; storing state alone does not mean the model sees it.

This is source inspection, not an executed repro, and the API details above are for v3.0.11.

## 2. #7665 — output review у вложенного Condition

Адрес публикации: https://github.com/agno-agi/agno/discussions/7665

Почему полезно: подтверждает точную границу async execution, отделяет Step output-review от executor HITL и даёт минимальную перестановку. Не обещает исправление upgrade; не обобщает неподтверждённо на все wrappers. Runtime не запускался.

### Точный английский текст

Your `Condition` finding matches the **v2.6.0 async path**. [`Workflow._aexecute`](https://github.com/agno-agi/agno/blob/v2.6.0/libs/agno/agno/workflow/workflow.py#L2957-L2978) runs output-review checks after a top-level `Step` or `Router`. [`Condition.aexecute`](https://github.com/agno-agi/agno/blob/v2.6.0/libs/agno/agno/workflow/condition.py#L927-L977) calls child steps directly and only propagates a result that is **already paused**; it does not evaluate each child's output-review configuration.

A custom executor returning a normal `StepOutput` therefore has no pause for the `Condition` to propagate. This is separate from an agent/tool executor pausing for its own HITL requirement.

The narrow workaround is to make `Need_Review` a direct item in `Workflow.steps`, before the downstream step, with the existing executor and `HumanReview` configuration. Use a workflow database, inspect `steps_requiring_output_review` on the paused result, resolve that requirement, then resume the same run with `acontinue_run`. [Documented output-review lifecycle](https://docs.agno.com/workflows/hitl/output-review).

If the original condition must remain, simply lifting the step changes its semantics. Keep the branch predicate in a top-level executor and explicitly distinguish “work ran” from “branch skipped”; use a callable `requires_output_review` to review only the former, and preserve the downstream branch's gating. v2.6.0 [supports that callable](https://github.com/agno-agi/agno/blob/v2.6.0/libs/agno/agno/workflow/utils/hitl.py#L232-L280), but an exception inside it **defaults to no review**, so it must handle every possible output shape.

Putting output review on the `Condition` itself is [rejected by its validator](https://github.com/agno-agi/agno/blob/v2.6.0/libs/agno/agno/workflow/types.py#L198-L206). Also, your `OnTimeout.approve` explicitly allows automatic approval; use a different policy if human approval is mandatory, and keep irreversible actions after the review boundary.

The same top-level check is still present in [v3.0.11](https://github.com/agno-agi/agno/blob/v3.0.11/libs/agno/agno/workflow/workflow.py#L4123-L4143), so upgrading alone is not an established fix for this shape. This is code inspection, not a runtime repro; I have not verified every other composite primitive or resume path.

## Проверки перед отправкой

- [x] Тексты тредов и все существующие replies прочитаны.
- [x] Исходники закреплены тегами; текущий релиз проверен.
- [x] В текстах нет утверждения о выполненном runtime-тесте.
- [x] Нет просьб принять ответ, поставить реакцию или помочь получить achievement.
- [x] Локальный синтез исходников завершён; факт остановки трёх Grok-прогонов до финальных ответов отражён выше.
- [x] Line anchors сопоставлены с загруженными тегами; parent повторно подтвердил отсутствие replies и блокировки. Исполняемых примеров нет.
- [ ] Владелец явно разрешил отправить конкретные тексты.
