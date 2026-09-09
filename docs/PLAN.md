# Implementation Plan

Status: `WAITING_FOR_INTAKE`

## Strategy

Deliver one working Must vertical slice first, finish Must scope, stabilize, then spend remaining time only on high-value judged/demo work.

Current phase/timebox is owned by `workflow/STATE.md`.

## Phase strategy

| Phase | Exit condition |
|---|---|
| BUILD | first working Must vertical slice |
| MUST_COMPLETE | remaining Must closed |
| STABILIZE | integration/delivery/reliability acceptable |
| FEATURE_FREEZE | no new features |
| DEMO_REHEARSAL | reproducible demo/evidence/submission ready |
| SUBMISSION | preserve known-good state and submit |

Adapt relative timing to the actual event duration; do not invent clock timestamps.

## Slices

| Slice | Goal | User-visible result | Status |
|---|---|---|---|
| S1 | TODO | TODO | NOT_STARTED |
| S2 | TODO | TODO | NOT_STARTED |

Allowed: `NOT_STARTED | ACTIVE | DONE | DROPPED`.

## First runnable slice

The first runnable slice establishes the resolved delivery path. If `CI_REQUIRED:YES`, it also establishes minimal task-specific CI.

If it introduces or depends on `USE_AI`, provider viability is front-loaded rather than deferred: before substantive AI-dependent implementation, perform the bounded same-runtime provider preflight from `docs/AI.md`, confirm current model/network route, then either continue on PASS or execute the recorded contingency within the provider-debug budget. A provider failure discovered only during FINALIZE/DEMO_REHEARSAL is a planning failure unless the provider was genuinely unavailable earlier.

The slice also establishes the provider boundary, safe secret/config contract, validation/fallback and representative evaluation.

## Freeze rule

After FEATURE_FREEZE: no new features, late AI addition, CI beautification/coverage work, Docker optimization or cosmetic redesign. Only Must/official-gate fixes, reliability, delivery, evidence, demo and submission work.
