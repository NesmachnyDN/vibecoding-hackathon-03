# AGENTS.md

Repository-wide rules for Codex and coding agents.

## 1. Scope

This is a solo hackathon repository. Implement only the active contract in `workflow/NEXT_CODEX_TASK.md`; optimize for a demonstrable Must result, small diffs and fast verification.

Do not stop after analysis unless a genuine blocker exists.

## 2. Context budget — task scoped by default

For every ordinary IMPLEMENT/FIX/CONTINUE run, preload only the execution core:

1. `AGENTS.md`
2. `workflow/STATE.md`
3. `workflow/NEXT_CODEX_TASK.md`

Then read only files/sections explicitly listed under the task's `## Context pack`, plus the smallest source/test files needed to execute the change.

Do **not** preload `README.md`, `docs/SPEC.md`, `docs/PLAN.md`, `docs/QUALITY.md`, `docs/DELIVERY.md`, `docs/AI.md`, `docs/EVIDENCE.md` or `docs/SUBMISSION.md` merely because they exist. The active task must carry the task-relevant acceptance criteria and resolved contract snapshot needed for normal execution.

A context-pack reference may name a whole file or a heading/range. Prefer the narrowest useful slice. Do not recursively open adjacent process documents unless the task requires them.

If a concrete execution-critical fact is missing, read the **single** canonical document that owns that fact, note the context-pack gap in the result, and continue when safe. Do not sweep the whole `docs/` directory.

FINALIZE is intentionally broader: read the finalization context named by the task and, at minimum, `docs/QUALITY.md`, `docs/EVIDENCE.md` and `docs/SUBMISSION.md`.

## 3. Repository topology and orchestration freshness

Topology lives in `workflow/STATE.md`, but the active task must never be trusted from an arbitrary stale checkout.

Before reading `workflow/NEXT_CODEX_TASK.md`, every launcher-driven Codex run must follow the pre-run freshness gate in `prompts/CODEX-RUN.md`: clean-state check, safe fetch of the control-plane `main`, non-rewriting refresh of local `main`, then exact `EXPECTED_TASK_ID == STATE.CURRENT_TASK_ID == NEXT.TASK_ID` validation. Dirty state, diverged/ahead local main, unsafe history or task-ID mismatch is `BLOCKED`; never repair it with reset/rebase/force-push.

- `GITHUB_PRIMARY`: `origin` is official and `origin/main` is the orchestration source.
- allowed mirror mode: official GitLab/V-Works remains `origin`; GitHub control plane is `github`, and `github/main` carries orchestration updates.
- `*_ONLY`: do not create an external mirror.

If `REMOTE_SETUP_STATUS=PENDING_CODEX_SETUP`, `prompts/CODEX-RUN.md` performs the one-time non-rewriting remote setup/sync before product work.

Never force-push/rebase/reset valid official history merely to create parity.

## 4. Task boundaries and architecture

Implement the smallest complete slice. No unrelated refactor, speculative shared abstraction, framework/runtime/database/container/service or cosmetic UI work without a Must/official/security/reproducibility reason.

Preserve clear ownership, dependency direction and external integration boundaries. Keep provider-specific details out of domain/application logic when a narrow adapter is sufficient.

## 5. Iteration quality baseline

For IMPLEMENT/FIX/CONTINUE, the default quality gate is intentionally compact and does not require loading the full quality document:

- satisfy every active acceptance criterion;
- run every task `Required checks` item and relevant existing focused tests;
- do not claim an unrun check as PASS;
- preserve security/data-integrity boundaries and do not commit secrets;
- avoid unrelated scope, dead code and obvious regression;
- keep the resolved delivery/AI/UI/CI contract intact;
- commit/push the required branch/remotes and verify required parity.

Read `docs/QUALITY.md` during an ordinary iteration only when the task Context pack explicitly lists it because the change is architecture/security/quality-sensitive or otherwise needs the detailed policy.

FINALIZE always uses the full FINAL gate in `docs/QUALITY.md`.

## 6. Resolved delivery, CI and AI contracts

Use the task's `## Resolved contract snapshot` as the execution-time cache of the current task-relevant decisions. The Chat orchestration stage owns keeping that snapshot aligned with canonical project state.

Read `docs/DELIVERY.md` or `docs/AI.md` only when explicitly listed in the Context pack or when a concrete missing fact prevents safe execution.

Project CI belongs to the official primary platform and is created only when the resolved task contract requires it, normally with the first runnable slice. It is verification-only unless official rules require more.

Never fake AI. `NO_AI` / `PROHIBITED` means no model dependency; `BLOCKED` means do not simulate mandatory AI; `USE_AI` must preserve data/secret boundaries and bounded output validation.

## 7. Evidence and submission are lazy context

Do not preload evidence/submission state for ordinary implementation.

Read/update `docs/EVIDENCE.md` only at the evidence step when the completed change creates or changes a demonstrable fact. Read `docs/SUBMISSION.md` only for FINALIZE/submission work or when the active task explicitly lists it.

Unrun checks are never PASS. Record exact commands/scenarios actually executed when evidence is updated.

## 8. Git completion

Complete task → commit/push required branch/remotes → verify required parity → stop. Review selects the next action.
