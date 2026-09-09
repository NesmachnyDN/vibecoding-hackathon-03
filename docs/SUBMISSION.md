# Submission

SUBMISSION_STATUS: NOT_READY
PROJECT_NAME: Контроль проектных рисков
OFFICIAL_PRIMARY_REPOSITORY: NesmachnyDN/vibecoding-hackathon-03
DELIVERABLE_LANGUAGE: RU
FINAL_REF: NOT_SET
FINAL_COMMIT_SHA: NOT_SET
FINAL_CONTENT_SHA: NOT_SET
SUBMISSION_TIMESTAMP: NOT_SET
OFFICIAL_SUBMISSION_MECHANISM: UNKNOWN_FROM_TASK
CI_REQUIRED: NO
CI_STATUS: N/A
OFFICIAL_PRIMARY_SHA: NOT_SET

Allowed status: `NOT_READY | READY | BLOCKED`.

## Required artifacts

| Artifact | Required? | Final location/status |
|---|---|---|
| Source repository / source code | YES | NesmachnyDN/vibecoding-hackathon-03 / NOT_VERIFIED |
| README with launch instructions | YES | `README.md` / draft intake contract only |
| Runnable application | YES | planned `index.html` / NOT_CREATED |
| Working demo scenario | YES | NOT_VERIFIED |
| Architecture description | YES | README/SPEC contain design decision; final description NOT_VERIFIED |
| Risk scoring algorithm description | YES | `docs/SPEC.md` contains fixed contract; runtime match NOT_VERIFIED |
| Presentation for judges | YES | NOT_CREATED |

## Final run/demo

- DELIVERY_PROFILE: STATIC_SINGLE_FILE
- Canonical artifact/open/start command: target `index.html` direct-open / NOT_VERIFIED
- Demo happy path/result: NOT_SET
- CI evidence: N/A by resolved contract
- Demo rehearsed: NO

## Final compliance

- Must/hard gates: core must work without external AI; CSV/demo data, scoring/dashboard/filter/detail/recommendation required; README + architecture + algorithm + presentation required.
- Required disclosures/licenses: UNKNOWN / no external dependency planned.
- Repository sync/parity: NOT_VERIFIED
- No secrets/confidential data: final verification required; task explicitly forbids storing API keys/tokens in repository.

## Finalize protocol

1. Complete product + factual evidence and commit accepted content baseline.
2. Record that commit as `FINAL_COMMIT_SHA` / `FINAL_CONTENT_SHA` and final ref/timestamp facts.
3. Fill only evidenced submission facts.
4. Apply FINAL gate from `docs/QUALITY.md`.
5. Set READY only when final acceptance passes.
6. Any later product behavior change invalidates the recorded baseline and requires FINALIZE again.

## Presentation contract

Presentation is mandatory. When final verdict is READY, generate a concise 4–5 slide 16:9 deck in Russian from SPEC/EVIDENCE/current product visuals.

Main narrative:

1. проблема руководителя и целевой пользователь;
2. рабочий сценарий + видимый результат портфельного анализа;
3. прозрачный алгоритм риска + минимальная архитектура/способ запуска; AI — только если реально работает и релевантен;
4. практическая ценность и соответствие критериям жюри;
5. продуктовый итог и запоминаемый пользовательский эффект.

Closing slide = product outcome, not project status. Не выносить на основные слайды внутренние статусы, test counts, SHA, команды CI или незавершённый backlog, если это прямо не требуется жюри.
