# Evidence

Keep only factual current implementation/verification facts. Planned capability is not evidence.

## Product

- Problem/user: по официальному заданию руководителю нужен быстрый обзор проектных рисков и приоритетных действий.
- Demo-critical flow: официально требуется показать запуск, данные, классификацию, панель, фильтрацию, критический проект, причины оценки, рекомендацию и опциональный AI при наличии доступа.
- Observed result: NOT_VERIFIED — product implementation ещё не создана.

## Capabilities

| Capability | Evidence | Status |
|---|---|---|
| Детерминированная оценка риска | Контракт зафиксирован в SPEC, runtime отсутствует | NOT_STARTED |
| Панель руководителя | План/AC зафиксированы, runtime отсутствует | NOT_STARTED |
| CSV upload + filters | План/AC зафиксированы, runtime отсутствует | NOT_STARTED |
| Карточка + deterministic action | План/AC зафиксированы, runtime отсутствует | NOT_STARTED |
| Optional AI recommendation | Provider/browser readiness не запускался | NOT_STARTED |

Allowed status: `WORKING | PARTIAL | NOT_VERIFIED | NOT_STARTED`.

## Verification

| Check | Exact command/scenario | Result/ref |
|---|---|---|
| Happy path | N/A before implementation | NOT_RUN |
| Negative/edge | N/A before implementation | NOT_RUN |
| Tests/build/lint/typecheck | N/A before implementation | NOT_RUN |

## AI

- AI_USAGE_DECISION: USE_AI, optional only; Must remains deterministic.
- Use case/provider/contour: short management recommendation; operator-configurable OpenAI-compatible browser-direct path if verified.
- Validation/evaluation/fallback evidence: contract only; live provider check NOT_RUN; deterministic fallback required.
- Observed AI result: NOT_RUN / no provider success claim.

## Delivery / CI

- DELIVERY_PROFILE: STATIC_SINGLE_FILE (resolved design decision, not yet verified runtime).
- Canonical artifact/start path: planned `index.html` direct-open; file does not yet exist.
- Delivery lifecycle evidence: NOT_RUN
- CI_REQUIRED: NO
- CI_STATUS: N/A
- CI evidence/ref: N/A

## Security / robustness

- Relevant controls verified: NOT_VERIFIED; contract requires optional token to remain memory-only and uncommitted.

## Repository sync

- Mode/primary/mirror: GITHUB_PRIMARY / `NesmachnyDN/vibecoding-hackathon-03` / no mirror.
- Relevant branch/main parity: initialization/intake writes use official `main`; implementation branch not created yet.

## Known limitations

- No product/runtime implementation exists yet.
- Optional AI provider/browser/CORS/network/model readiness is NOT_RUN and must not be treated as PASS.
