# Гонка duplicate PR

Использовать, когда несколько PR чинят один upstream outage.

## Субагенты

Запускать параллельно:

1. **Diff coverage**: файлы, runtime paths, helper functions, поведение.
2. **Test coverage**: regression tests, fixtures, CI, validation claims.
3. **Maintainer signal**: labels, comments, reviews, timeline, linked issues, reactions.

## Решение

После синтеза сделать одно:

- портировать недостающее поведение/тесты в canonical PR;
- спросить maintainers, какой PR должен стать canonical;
- написать duplicate-check update, что extra behavior не найдено.

## Формат комментария

```text
Duplicate check update: I compared the newer duplicate PRs #A and #B.

- #A covers the same parser guard and adds X regression.
- #B covers the same minimal guard.

#CURRENT already includes the parser guard/regression and additionally covers Y, so I did not find extra behavior from those PRs that needs to be pulled into this branch.
```

