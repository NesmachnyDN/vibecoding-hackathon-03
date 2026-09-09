# Product Specification

Status: `NOT_STARTED`

## Task and user

- Official task: TODO
- Primary user: TODO
- Problem: TODO
- Observable outcome: TODO

## Demo-critical happy path

1. TODO
2. TODO
3. TODO

## Scope

### Must
- TODO

### Should
- TODO

### Could
- TODO

### Won't
- TODO

## Acceptance criteria

| ID | Observable criterion | Verification |
|---|---|---|
| AC-001 | TODO | TODO |

## Official constraints

- Required/forbidden technologies: TODO / UNKNOWN
- Submission/hard gates: TODO / UNKNOWN
- Data/security restrictions: TODO / UNKNOWN
- Event deadline/duration evidence: TODO / UNKNOWN

## Deliverable language

- DELIVERABLE_LANGUAGE: TODO: RU / EN
- LANGUAGE_DECISION_SOURCE: TODO: DEFAULT_RU / OFFICIAL_RULES / TASK_REQUIREMENT
- LANGUAGE_EVIDENCE: TODO

Rule: explicit official/task requirement overrides; otherwise RU.

## Execution environment preflight

Record only material facts; UNKNOWN is valid when non-blocking. Never store secret values.

| Factor | State / evidence |
|---|---|
| Browser/OS/runtime constraints | UNKNOWN |
| Docker/Compose | UNKNOWN |
| Internet/registries | UNKNOWN |
| Official repository/CI capability | GITHUB / UNKNOWN |
| External/internal API reachability | UNKNOWN |
| Direct browser API viability (provider policy/CORS/origin) | UNKNOWN / N/A |
| Network route for external APIs (direct/TUN/env proxy/internal) | UNKNOWN |
| Proxy/VPN/runtime transport compatibility | UNKNOWN |
| Data-transfer restrictions | UNKNOWN |
| Required credential/service presence | UNKNOWN |

ENVIRONMENT_BLOCKERS: none / TODO

## AI decision

Resolve using `docs/AI.md` before final stack/delivery choice.

- AI_USAGE_DECISION: TODO: USE_AI / NO_AI / PROHIBITED / BLOCKED
- Deterministic baseline: TODO
- AI_USE_CASE: TODO / N/A
- AI_VALUE_OVER_DETERMINISTIC: TODO / N/A
- AI_EXECUTION_CONTOUR: TODO / N/A
- AI_PROVIDER_PROFILE: TODO / N/A
- AI_REUSE_POLICY/SOURCE: TODO / N/A
- AI_CREDENTIAL_MODE: TODO: NONE / OPERATOR_SESSION_BYOK / SERVER_SIDE_SECRET / OFFICIAL_BROWSER_MECHANISM / INTERNAL_ENDPOINT_AUTH / N/A
- AI_SECRET_HANDLING: TODO / N/A
- AI_OUTPUT_VALIDATION: TODO / N/A
- AI_EVALUATION/FALLBACK: TODO / N/A
- AI_PROVIDER_READINESS: TODO: PASS / DEGRADED / FAIL / NOT_RUN / N/A
- AI_BROWSER_DIRECT_READINESS: TODO: PASS / FAIL / NOT_RUN / N/A
- AI_NETWORK_PATH: TODO: DIRECT / SYSTEM_TUNNEL / ENV_PROXY / INTERNAL / UNKNOWN / N/A
- AI_MODEL_STATUS: TODO: CURRENT / REPLACED / UNKNOWN / N/A
- AI_PREFLIGHT_EVIDENCE: TODO / N/A
- AI_PROVIDER_CONTINGENCY: TODO / N/A
- AI_PROVIDER_DEBUG_BUDGET_MIN: TODO / N/A

Rule for `USE_AI`: configured endpoint/key/model is not readiness. Prefer a minimal live inference preflight through the same runtime/client/network path as the application. If the secret cannot be used during intake, mark `NOT_RUN` and make provider viability the first gate of the first AI-dependent Codex task. AI-mandatory + no compliant live path must surface early as a blocker; optional AI must have a useful contingency/fallback.

Credential presence alone is not a backend requirement. If the Must scope otherwise fits a browser-only artifact, evaluate `OPERATOR_SESSION_BYOK` or an official browser mechanism before selecting `LOCAL_APPLICATION`. Browser-direct AI requires explicit policy/CORS/origin/data-transfer PASS from the exact intended browser delivery path. Shared/long-lived hidden secrets, browser-policy/CORS failure or another genuine server responsibility justify backend promotion.

## Stack and delivery

Resolve jointly after environment + AI decision.

- STACK_DECISION: TODO
- EXECUTABLE_ARTIFACT: TODO: YES / NO
- DELIVERY_PROFILE: TODO: STATIC_SINGLE_FILE / LOCAL_APPLICATION / CONTAINERIZED / TASK_OVERRIDE / N/A
- DELIVERY_RATIONALE: TODO
- STATIC_BROWSER_API_EVIDENCE: TODO: PASS summary / FAIL reason / NOT_RUN / N/A
- Simpler profile rejected because: TODO / N/A
- COMMAND_FACADE: TODO: DIRECT_OPEN / DIRECT_COMMAND / MAKE / N/A
- Canonical build/open/start/stop/test: TODO / N/A
- Required config names/ports/state: TODO / N/A

For `LOCAL_APPLICATION`, “AI needs a token” is not sufficient `DELIVERY_RATIONALE` by itself. Name the real server responsibility or failed/forbidden browser condition. For `STATIC_SINGLE_FILE` + `OPERATOR_SESSION_BYOK`, document that the token is operator-supplied, memory-only, non-persistent and re-entered after reload/close; do not store its value anywhere.

## Project CI

- CI_REQUIRED: TODO: YES / NO / N/A
- CI_PLATFORM: TODO: GITHUB_ACTIONS / GITLAB_CI / VWORKS / OTHER / N/A
- CI_CONFIG_PATH: TODO / N/A
- CI_REQUIRED_CHECKS: TODO / N/A
- Decision evidence/reason: TODO

Official primary selects CI platform; mirror convenience never changes ownership.

## UI contract

- UI_REQUIRED: TODO: YES / NO
- Primary surface/user task: TODO / N/A
- Demo viewport/device: TODO / N/A
- Visual direction/design system: TODO / N/A
- Required states/accessibility/responsive expectations: TODO / N/A
- AI settings UX when applicable: TODO / N/A; for `OPERATOR_SESSION_BYOK`, masked credential input + explicit connection check/status, no secret echo/persistence.

## Open blockers

- none
