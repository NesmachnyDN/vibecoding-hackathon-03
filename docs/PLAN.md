# Implementation Plan

Status: `ACTIVE`

## Strategy

Сначала получить работающий детерминированный вертикальный срез без внешних зависимостей. Затем закрыть CSV/фильтры и устойчивость, после чего использовать оставшееся время только на опциональный AI при подтверждённой доступности, UI-полировку и финальную демонстрацию.

Current phase/timebox is owned by `workflow/STATE.md`; абсолютный дедлайн не предоставлен.

## Phase strategy

| Phase | Exit condition |
|---|---|
| BUILD | первый работающий Must vertical slice в одном `index.html` |
| MUST_COMPLETE | CSV, фильтры, детальная карточка и error paths закрывают все Must AC |
| STABILIZE | повторный запуск, ошибки ввода, UI и optional-AI contingency проверены |
| FEATURE_FREEZE | никаких новых функций |
| DEMO_REHEARSAL | README/evidence/demo/submission/presentation готовы |
| SUBMISSION | сохранено проверенное состояние и выполнена сдача |

## Slices

| Slice | Goal | User-visible result | Status |
|---|---|---|---|
| S1 / I001 | Первый runnable dashboard + scoring + detail + ранний optional-AI readiness surface | Демоданные сразу дают объяснимый портфельный обзор; критический проект можно открыть; AI connection state не блокирует core | DONE |
| S2 / I002 | CSV upload, risk/stage filters, validation/error recovery | Пользователь загружает свой файл и быстро отбирает проблемные проекты | ACTIVE |
| S3 | Optional AI recommendation only if verified; otherwise deterministic polish | Дополнительная AI-рекомендация либо уверенный fallback без провайдера | NOT_STARTED |
| S4 | Stabilize/finalize/demo/presentation | Воспроизводимый запуск, доказательства и убедительный сценарий для жюри | NOT_STARTED |

Allowed: `NOT_STARTED | ACTIVE | DONE | DROPPED`.

## First runnable slice

`I001` завершён: self-contained `index.html` реализует фиксированный алгоритм риска, демоданные, KPI/распределение/top проблемных проектов, таблицу и карточку проекта с детерминированным действием. Optional AI connection surface сохраняет deterministic core; live provider readiness остаётся NOT_RUN и не считается PASS.

`I002` закрывает оставшийся Must scope по пользовательским данным: атомарный CSV import, validation/error recovery, возврат к демоданным и совместные risk/stage filters. После успешного I002 следующий review должен оценить, закрыты ли все Must/hard gates и пора ли переходить к стабилизации/finalize вместо автоматического добавления optional AI.

## Freeze rule

После FEATURE_FREEZE: без позднего AI, новых функций, CI, Docker-оптимизации и косметического редизайна. Только обязательные исправления, надёжность, запуск, evidence, demo и submission.
