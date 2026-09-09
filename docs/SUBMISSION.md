# Submission

SUBMISSION_STATUS: NOT_READY
PROJECT_NAME: Контроль проектных рисков
OFFICIAL_PRIMARY_REPOSITORY: NesmachnyDN/vibecoding-hackathon-03
DELIVERABLE_LANGUAGE: RU
FINAL_REF: main
FINAL_COMMIT_SHA: f8645473c7c61b00e4ada0bcc9027d900a708db9
FINAL_CONTENT_SHA: f8645473c7c61b00e4ada0bcc9027d900a708db9
SUBMISSION_TIMESTAMP: 2026-09-10T00:40:18+07:00
OFFICIAL_SUBMISSION_MECHANISM: UNKNOWN_FROM_TASK
CI_REQUIRED: NO
CI_STATUS: N/A
OFFICIAL_PRIMARY_SHA: f8645473c7c61b00e4ada0bcc9027d900a708db9

Allowed status: `NOT_READY | READY | BLOCKED`.

## Required artifacts

| Artifact | Required? | Final location/status |
|---|---|---|
| Source repository / source code | YES | `main` / content baseline `f8645473c7c61b00e4ada0bcc9027d900a708db9` / VERIFIED |
| README with launch instructions | YES | `README.md` / first-run + subsequent-run VERIFIED |
| Runnable application | YES | корневой `index.html` / VERIFIED direct-open |
| Working demo scenario | YES | VERIFIED in Chrome 153 at 1366×768 and 1024×768 |
| Architecture description | YES | `README.md` + `docs/SPEC.md` / VERIFIED current scope |
| Risk scoring algorithm description | YES | `README.md` + `docs/SPEC.md` / runtime exact fixture match VERIFIED |
| Presentation for judges | YES | NOT_CREATED / PENDING_CHAT_GENERATION |

## Final run/demo

- DELIVERY_PROFILE: STATIC_SINGLE_FILE
- Canonical artifact/open/start command: `xdg-open index.html` from repository root / VERIFIED twice from clean committed checkout
- Demo happy path/result: scores `46/3/100/22/58/0`, KPI `6/3/2`, distribution `2/1/1/2`, установленный top-3, valid/invalid CSV, filters/zero/restore and critical detail / PASS
- CI evidence: N/A by resolved contract
- Demo rehearsed: YES

## Final compliance

- Must/hard gates: product/README/architecture/algorithm/demo PASS; mandatory presentation remains PENDING_CHAT_GENERATION, therefore `SUBMISSION_STATUS` remains `NOT_READY`.
- Required disclosures/licenses: N/A — no external runtime dependencies, libraries, fonts, images or copied product assets.
- Repository sync/parity: content baseline push/fetch PASS at `f8645473c7c61b00e4ada0bcc9027d900a708db9`; final metadata commit parity is recorded in I003 handoff after push.
- No secrets/confidential data: PASS — synthetic fixture only; high-confidence secret scan clean; CSV remains local and runtime has no storage/log/network path.

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
