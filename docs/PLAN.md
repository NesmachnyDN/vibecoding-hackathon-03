# Implementation Plan

Status: `DONE`

## Strategy

Сначала получен работающий детерминированный вертикальный срез без внешних зависимостей, затем закрыты CSV/фильтры и устойчивость. Must scope завершён; стабилизация, воспроизводимость, evidence, demo и submission выполнены без добавления новых функций.

Current phase/timebox is owned by `workflow/STATE.md`; абсолютный дедлайн не предоставлен.

## Phase strategy

| Phase | Exit condition |
|---|---|
| BUILD | первый работающий Must vertical slice в одном `index.html` |
| MUST_COMPLETE | CSV, фильтры, детальная карточка и error paths закрывают все Must AC |
| STABILIZE | повторный запуск, ошибки ввода и UI contingency проверены |
| FEATURE_FREEZE | никаких новых функций |
| DEMO_REHEARSAL | README/evidence/demo/submission/presentation готовы |
| SUBMISSION | сохранено проверенное состояние и выполнена сдача |

## Slices

| Slice | Goal | User-visible result | Status |
|---|---|---|---|
| S1 / I001 | Первый runnable dashboard + scoring + detail | Демоданные дают объяснимый портфельный обзор; критический проект можно открыть | DONE |
| S2 / I002 | CSV upload, risk/stage filters, validation/error recovery | Пользователь загружает свой файл и быстро отбирает проблемные проекты | DONE |
| S3 | Optional AI recommendation | Не входит в финальный scope: принят `AI_USAGE_DECISION=NO_AI`, итоговая рекомендация детерминирована | DROPPED |
| S4 / I003 | Stabilize/finalize/demo/submission preparation | Воспроизводимый deterministic продукт, фактические доказательства и презентация готовы к submission | DONE |

Allowed: `NOT_STARTED | ACTIVE | DONE | DROPPED`.

## Finalization strategy

`I003` выполнен на `main` без новых функций. Незавершённая optional-AI connection surface удалена, canonical AI decision приведён к `NO_AI`; clean-checkout first/subsequent run, integrated Must regressions, security/UI checks и factual submission/evidence update выполнены.

Финальный Chat review применил FINAL gate и сформировал обязательную 5-слайдовую 16:9 презентацию на русском языке.

## Freeze rule

С I003 действует feature freeze: без позднего AI, новых функций, CI, Docker-оптимизации и косметического редизайна. Только обязательные исправления, надёжность, запуск, evidence, demo и submission.
