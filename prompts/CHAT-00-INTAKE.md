# CHAT-00 — Intake

Use when the repository was prepared before the official task became available. The initial start flow may execute the same logic automatically when the task is already supplied.

```text
Work with GitHub API repository NesmachnyDN/vibecoding-hackathon-03. Intake is a one-time orchestration step, so you may read the compact runtime baseline needed to resolve the task: STATE, AGENTS, README, SPEC, PLAN, QUALITY, DELIVERY, AI, EVIDENCE, SUBMISSION and NEXT task.

Goal: convert the official rules/task into exactly one bounded, **self-contained** first Codex task. Do not implement product code yourself.

1. Confirm topology/access from STATE. In allowed mirror mode, PENDING_CODEX_SETUP is not an intake blocker: first CODEX-RUN will configure/sync remotes before product work.
2. Parse official task literally: user/problem, Must/Should/Could/Won't, inputs/outputs, hard gates, required/forbidden technology, judging/submission/data constraints. Official rules override defaults.
3. Resolve deliverable language: explicit requirement wins; otherwise RU.
4. Record demo-critical happy path and stable observable AC IDs.
5. Resolve timebox from evidence only; do not invent timestamps.
6. Run only material environment preflight: runtime/browser/Docker when relevant, internet/registries, official CI capability, required APIs/services/credentials, data-transfer restrictions, and the actual network route for external APIs. If VPN/proxy may be involved, record whether the intended runtime uses direct networking, a system tunnel/TUN, environment proxy variables, an explicit proxy, or an internal route. UNKNOWN is valid when non-blocking.
7. Resolve AI necessity before final stack/delivery choice. For `USE_AI`, apply the provider-readiness gate from `docs/AI.md` **early**:
   - configured endpoint/key/model or success in another application is not provider evidence;
   - determine `AI_CREDENTIAL_MODE`: NONE / OPERATOR_SESSION_BYOK / SERVER_SIDE_SECRET / OFFICIAL_BROWSER_MECHANISM / INTERNAL_ENDPOINT_AUTH;
   - credential presence alone is not a backend requirement;
   - prefer a minimal live inference request from the same runtime/client/network path the project will use;
   - verify credential acceptance, current model availability and required structured-output mode when applicable;
   - never print/store secret values;
   - record `AI_PROVIDER_READINESS`, `AI_BROWSER_DIRECT_READINESS`, `AI_NETWORK_PATH`, `AI_MODEL_STATUS`, safe preflight evidence, contingency and bounded debug budget;
   - if a live secret/path is unavailable to Chat during intake, set readiness `NOT_RUN` and make the provider viability check the **first executable gate** of the first AI-dependent Codex task;
   - if AI is mandatory and no compliant live path exists, surface `BLOCKED` before building an AI-dependent happy path;
   - if AI is optional, preserve a useful deterministic/provider contingency and do not let provider setup consume the hackathon.
8. Resolve the least-complex stack/delivery profile using this explicit anti-overengineering branch **before** choosing Python/Node/backend merely for token storage:
   - official required delivery mechanism still wins;
   - if complete Must fits the browser and no credential is needed, prefer `STATIC_SINGLE_FILE`;
   - if complete Must fits the browser and AI/API needs a credential, evaluate `OPERATOR_SESSION_BYOK` or an official browser mechanism before introducing a backend;
   - `OPERATOR_SESSION_BYOK` is acceptable only when rules/data policy/provider browser terms allow it, the operator knowingly supplies their own temporary credential, it stays page-memory-only (no source/config/localStorage/IndexedDB/cookies/URL/logs/screenshots), and re-entry after reload/close is acceptable;
   - for a browser-direct candidate, preflight the **exact intended browser delivery path** (direct file or chosen static origin), not curl/Python: verify provider browser policy, CORS/origin, network route, credential acceptance, current model and required response mode using synthetic data;
   - browser-direct PASS + no real server responsibility → `STATIC_SINGLE_FILE`;
   - browser-direct FAIL/forbidden, shared/long-lived hidden application secret, server-side auth/state/file processing/database/background work or another genuine server responsibility → `LOCAL_APPLICATION` when containers add no value;
   - multiple services/system dependencies/fragile host setup may justify `CONTAINERIZED` when permitted;
   - “AI needs a token” is **not** sufficient rationale for rejecting `STATIC_SINGLE_FILE`; record the concrete failed/forbidden browser condition or real server responsibility in `DELIVERY_RATIONALE` / `Simpler profile rejected because`.
9. Resolve CI contract and minimal UI contract. For `OPERATOR_SESSION_BYOK`, the UI contract must include a professional masked settings/input flow, explicit connection check/status, no secret echo/persistence, and understandable re-entry behavior. External-provider networking must be reproducible on the declared demo machine; do not silently depend on ambient proxy/VPN/shell state.
10. Update README, SPEC, PLAN, factual EVIDENCE, official SUBMISSION requirements, STATE and NEXT task. For an executable artifact, README must define a canonical first-run-from-clean-checkout path and a separate subsequent-run path. Commands must be runnable from the repository root without relying on prior shell activation, globally installed application packages/CLIs, IDE state, user aliases or uncommitted files. Project-managed runtime/tool paths or checked-in wrappers/Make targets are preferred. For Python LOCAL_APPLICATION on Linux, prefer `python3 -m venv .venv` plus direct `.venv/bin/python ...` commands rather than bare globally resolved `python`/`pip`/`uvicorn`/`pytest` commands. For `STATIC_SINGLE_FILE` + `OPERATOR_SESSION_BYOK`, README documents only the operator action and non-secret endpoint/model/token field names; never the token value.
11. Create exactly one first task I001, MODE=IMPLEMENT, QUALITY_GATE_PROFILE=ITERATION, branch `feat/i001-<slug>`.
12. If `AI_USAGE_DECISION=USE_AI` and `AI_PROVIDER_READINESS!=PASS`, I001 must begin with a bounded provider-readiness gate before substantive AI-dependent implementation. For a `STATIC_SINGLE_FILE` candidate this gate must execute in the target browser/origin and classify CORS/browser-policy separately from auth/model/network failures. Required checks must make the outcome explicit: PASS -> continue with the chosen profile; browser path FAIL/forbidden -> promote to the least-complex justified alternative; fail within debug budget with viable contingency -> execute contingency; AI-mandatory with no viable path -> stop as BLOCKED. Do not postpone this gate to a later iteration or FINALIZE.
13. Make NEXT task self-contained: copy the task-relevant resolved delivery/CI/AI/UI/language/hard-gate decisions into `Resolved contract snapshot`; include bounded in/out scope, AC and Required checks.
14. Build `Context pack` as an allowlist, not a reading list. Add only the canonical document slice and source/test entry points genuinely needed for I001 beyond AGENTS + STATE + NEXT. **Do not list every process document by habit.** If the task contract already contains the needed fact, do not add SPEC/PLAN/policy merely as background.
15. For an initial greenfield slice, Context pack may legitimately be `NONE` until source files exist; include a policy document only when the first implementation actually needs details not safely captured in the task snapshot.
16. Set STATE READY_FOR_CODEX with `CURRENT_TASK_ID=I001`, then re-read the committed NEXT on current `main` and verify `TASK_ID=I001` before returning the launcher. If AI is mandatory and provider viability is already known to be impossible, use `BLOCKED` instead of pretending the repository is ready.

Before returning the Codex launcher, verify current selectable Codex model/reasoning controls from official OpenAI Codex documentation, starting at `https://developers.openai.com/codex/models` and following its current official redirect/rendering. Generic API availability is not proof of Codex picker availability.

If verification succeeds, return a short `Codex execution recommendation (outside prompt)` with Model/Reasoning/Fallback/Why/source. If it fails, return recommendation UNAVAILABLE but still return the launcher. Never store model/reasoning metadata in NEXT task or the prompt.

Then return `Codex prompt — copy only this block` with the literal expected task ID:
«Работай с текущим локальным репозиторием. EXPECTED_TASK_ID=I001. Сначала выполни pre-run orchestration freshness gate из prompts/CODEX-RUN.md: безопасно синхронизируй control-plane main до чтения NEXT, проверь ветку и совпадение EXPECTED_TASK_ID с STATE/NEXT. Затем выполни актуальную workflow/NEXT_CODEX_TASK.md. Загрузи только core-контекст и Context pack задачи, не перечитывай все process docs. Не расширяй scope. Если task содержит AI provider-readiness или browser-direct readiness gate, выполни его первым и не продолжай AI-зависимую реализацию при неразрешённом blocker. Заверши после required verification, factual evidence refresh при необходимости, commit/push и required sync/parity check.»

--- TASK START ---
<PASTE OFFICIAL TASK>
--- TASK END ---
```
