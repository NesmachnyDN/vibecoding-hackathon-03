# Submission

SUBMISSION_STATUS: NOT_READY
PROJECT_NAME: Контроль проектных рисков
OFFICIAL_PRIMARY_REPOSITORY: NesmachnyDN/vibecoding-hackathon-03
DELIVERABLE_LANGUAGE: NOT_RESOLVED
FINAL_REF: NOT_SET
FINAL_COMMIT_SHA: NOT_SET
FINAL_CONTENT_SHA: NOT_SET
SUBMISSION_TIMESTAMP: NOT_SET
OFFICIAL_SUBMISSION_MECHANISM: UNKNOWN_FROM_RULES
CI_REQUIRED: NOT_RESOLVED
CI_STATUS: NOT_CONFIGURED
OFFICIAL_PRIMARY_SHA: NOT_SET

Allowed status: `NOT_READY | READY | BLOCKED`.

## Required artifacts

Populate only from official rules during intake/finalize.

| Artifact | Required? | Final location/status |
|---|---|---|
| Source repository | UNKNOWN | NesmachnyDN/vibecoding-hackathon-03 / NOT_VERIFIED |
| README | UNKNOWN | `README.md` / NOT_VERIFIED |
| Runnable artifact | UNKNOWN | NOT_SET |
| Presentation | UNKNOWN | NOT_SET |
| Other | UNKNOWN | NOT_SET |

## Final run/demo

- DELIVERY_PROFILE: NOT_RESOLVED
- Canonical artifact/open/start command: NOT_SET
- Demo happy path/result: NOT_SET
- CI evidence: NOT_SET
- Demo rehearsed: NO

## Final compliance

- Must/hard gates: UNKNOWN
- Required disclosures/licenses: UNKNOWN
- Repository sync/parity: NOT_VERIFIED / N/A
- No secrets/confidential data: NOT_VERIFIED

## Finalize protocol

1. Complete product + factual evidence and commit accepted content baseline.
2. Record that commit as `FINAL_COMMIT_SHA` / `FINAL_CONTENT_SHA` and final ref/timestamp facts.
3. Fill only evidenced submission facts.
4. Apply FINAL gate from `docs/QUALITY.md`.
5. Set READY only when final acceptance passes.
6. Any later product behavior change invalidates the recorded baseline and requires FINALIZE again.

## Presentation contract

When presentation is required and final verdict is READY, generate a concise 4–5 slide 16:9 deck from SPEC/EVIDENCE/current product visuals.

**Audience-first rule:** the main deck is for judges, not a shortened test report. `docs/EVIDENCE.md` constrains what may be claimed and supports Q&A, but low-level verification details must not become visible slide content by default.

Default narrative:

1. problem / target user / core idea;
2. solution flow + visible result;
3. concise architecture + delivery choice + functional AI only when `USE_AI` and judging-relevant;
4. **practical value / judge relevance / differentiator** — what changes for the user/process and why the solution deserves a high score under the official criteria;
5. **product outcome / memorable value takeaway** — what was created, what the user gets, practical effect, and the one product message judges should remember.

### Closing-slide rule

**Closing slide = product outcome, not project status.**

The default Slide 5 must not read like an internal handoff/status report to the participant. Do not use its visible content for:

- readiness/status language such as `готов к показу`, `готов к сдаче`, `MVP complete`, `READY`;
- lists of unfinished work or technical limitations;
- next steps / roadmap / what the participant should do after the hackathon;
- backlog, future integration plan or internal recommendations.

Keep those facts in README/EVIDENCE/SUBMISSION and Q&A material. Only surface limitations, risks, roadmap or next steps on the closing slide when official rules/judging explicitly require them. Even then, keep the primary closing message focused on the completed product and user value.

Do **not** make a main slide whose primary message is HTTP status codes, test counts, compile/lint commands, mock scenarios, repository SHA/parity or CI mechanics unless the official judging criteria explicitly require that technical evidence. Even then, translate it to a short outcome-level claim and keep detailed proof in `docs/EVIDENCE.md`, README, speaker notes or backup/Q&A material.

Examples:

- preferred visible claim: `Воспроизводимый локальный запуск подтверждён`;
- avoid as main-slide content: `17 passed`, `GET / = 200`, `POST = 422`, `compileall PASS`.

Use `DELIVERABLE_LANGUAGE` from SPEC. Never invent features, metrics, user feedback, CI/security/AI claims or repository status. Official judging weights control emphasis. Keep speaker notes roughly 20–45 seconds per slide. Delivery rationale should explain why the selected profile is the least-complex suitable option; do not treat Docker as proof of production readiness.
