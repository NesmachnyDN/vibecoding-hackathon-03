# CHAT-10 — Review / Recovery / Final Acceptance

Single orchestration prompt after any Codex run, including BLOCKED/PARTIAL and FINALIZE.

```text
Review the latest Codex iteration in GitHub API repository NesmachnyDN/vibecoding-hackathon-03 and prepare exactly one next action.

Always read STATE, current NEXT task and the actual target-branch/main diff. Read only the canonical project/process documents needed to judge the acceptance criteria or resolve the next task; EVIDENCE/SUBMISSION are mandatory only for final/submission work. Inspect repository facts rather than relying on a user/Codex summary.

1. Verify task/branch/scope, Required checks and required repository sync evidence.
2. For ordinary IMPLEMENT/FIX/CONTINUE apply the compact ITERATION baseline from AGENTS plus task Required checks. Read detailed QUALITY only for a concrete quality/architecture/security question that needs it; do not run the full FINAL checklist on every slice.
3. Check delivery/CI/UI/AI contracts only where relevant. Config presence is not CI PASS; configured endpoint is not AI PASS.
4. Classify mandatory defects: correctness/security/data loss/demo-critical/official gate = BLOCKER; required architecture/quality/delivery/CI/UI/AI/test/evidence/sync gap = MAJOR.
5. If Codex was BLOCKED/PARTIAL or a mandatory defect exists, choose FIX on the same branch with only root-cause actions and exact checks.
6. If PASS and a remaining Must fits the phase, choose CONTINUE with one next vertical slice.
7. If Must/hard gates are closed or phase requires stabilization, choose FINALIZE on main with QUALITY_GATE_PROFILE=FINAL and no new features.
8. Make every FIX/CONTINUE task self-contained: refresh `Resolved contract snapshot`, bounded scope, AC and Required checks from current canonical state.
9. Build the next `Context pack` from the actual change: normally relevant source/test files plus only the specific canonical doc slice required. Do not carry forward stale context entries and do not list SPEC/PLAN/QUALITY/DELIVERY/AI/EVIDENCE/SUBMISSION by habit.
10. FINALIZE is the exception: create an explicit broader Context pack including QUALITY, EVIDENCE, SUBMISSION and any SPEC/DELIVERY/AI sections needed to verify final hard gates.
11. When reviewing completed FINALIZE, apply the full FINAL gate. READY requires Must/hard gates, language, delivery, CI contract, applicable UI/AI, evidence/demo/submission and required repository sync all acceptable. Otherwise create one bounded FINALIZE fix task.
12. Near/after freeze never start late AI, optional feature breadth, CI beautification/coverage work, container micro-optimization or cosmetic redesign.
13. Update STATE/NEXT and only factual docs needed for the selected orchestration step. Do not edit product code yourself.
14. Before emitting any next Codex launcher, re-read the just-written NEXT from current `main`, capture its exact `TASK_ID`, and put that literal value into the launcher as `EXPECTED_TASK_ID=<id>`. Never emit a placeholder such as `<TASK_ID>` and never reuse an ID from the reviewed iteration unless NEXT actually contains it.

Before any next Codex launcher, verify current selectable model/reasoning controls from official OpenAI Codex documentation (`https://developers.openai.com/codex/models`, following the current official redirect). Generic API availability alone is insufficient.

If verification succeeds, output `Codex execution recommendation (outside prompt)`; if unavailable, mark recommendation UNAVAILABLE but still emit the model-free prompt.

For FIX/CONTINUE/FINALIZE return `Codex prompt — copy only this block` using the exact TASK_ID from the updated NEXT:
«Работай с текущим локальным репозиторием. EXPECTED_TASK_ID=<actual TASK_ID from refreshed workflow/NEXT_CODEX_TASK.md>. Сначала выполни pre-run orchestration freshness gate из prompts/CODEX-RUN.md: безопасно синхронизируй control-plane main до чтения NEXT, проверь ветку и совпадение EXPECTED_TASK_ID с STATE/NEXT. Затем выполни актуальную задачу из workflow/NEXT_CODEX_TASK.md. Для обычной итерации загрузи только core-контекст и Context pack задачи; для FINALIZE следуй расширенному finalization pack. Не расширяй scope. Заверши после required verification, factual evidence refresh при необходимости, commit/push и required sync/parity check.»

For READY, do not create another Codex task. Return the final submission checklist from `docs/SUBMISSION.md`; if presentation is required, generate it from the submission contract and factual evidence.
```
