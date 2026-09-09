# AI Usage Policy

AI is optional. Prefer the simplest reliable deterministic mechanism unless AI is explicitly required or adds observable material capability.

## Necessity gate

Before selecting AI:

1. Check official AI/provider/data-transfer/reuse rules independently.
2. Describe the simplest deterministic baseline.
3. Use AI only for a bounded semantic/generative/probabilistic capability where the deterministic baseline is insufficient or AI adds clear observable user/judging value.
4. Verify output can be evaluated and risk bounded by validation/grounding/human confirmation/fallback.
5. Verify a compliant reachable provider fits the environment/timebox.

Results:

- `USE_AI` — all gates pass.
- `NO_AI` — deterministic path is sufficient/preferable.
- `PROHIBITED` — explicit prohibition.
- `BLOCKED` — AI is mandatory but no compliant path exists.

AI is normally unjustified for exact calculations, CRUD, fixed filtering/sorting, known-schema validation, static formatting or a short deterministic rule set that already meets Must.

## Credential/trust mode

AI/API credential presence does **not** automatically imply a backend. Resolve one explicit mode before locking delivery:

- `NONE` — no credential required.
- `OPERATOR_SESSION_BYOK` — the local demo operator knowingly enters their own temporary credential into the UI for the current page/session.
- `SERVER_SIDE_SECRET` — a shared/long-lived/application credential must remain hidden from browser users; requires a server or approved gateway.
- `OFFICIAL_BROWSER_MECHANISM` — organizer/provider supplies a browser-safe delegated/auth mechanism.
- `INTERNAL_ENDPOINT_AUTH` — corporate/internal mechanism governs access.
- `N/A` — AI not used.

`OPERATOR_SESSION_BYOK` is allowed only when official rules, data policy and provider/browser terms permit direct client use. The credential must not be embedded in HTML/JS/config, committed, logged, screenshotted, placed in URLs, or persisted in `localStorage`, IndexedDB, cookies or similar storage. Keep it in page memory only; reload/close may require re-entry.

This is a hackathon/demo trust model, not a production credential architecture. When production hardening matters, state the intended server-side gateway, corporate gateway, short-lived delegated token or equivalent approved mechanism.

## Provider readiness gate

`USE_AI` requires early provider viability evidence. Do not postpone this to finalization or demo rehearsal.

Record:

- `AI_PROVIDER_READINESS: PASS | DEGRADED | FAIL | NOT_RUN | N/A`;
- `AI_CREDENTIAL_MODE: NONE | OPERATOR_SESSION_BYOK | SERVER_SIDE_SECRET | OFFICIAL_BROWSER_MECHANISM | INTERNAL_ENDPOINT_AUTH | N/A`;
- `AI_BROWSER_DIRECT_READINESS: PASS | FAIL | NOT_RUN | N/A`;
- `AI_NETWORK_PATH: DIRECT | SYSTEM_TUNNEL | ENV_PROXY | INTERNAL | UNKNOWN | N/A`;
- `AI_MODEL_STATUS: CURRENT | REPLACED | UNKNOWN | N/A`;
- `AI_PREFLIGHT_EVIDENCE: <safe summary without secret values>`;
- `AI_PROVIDER_CONTINGENCY: <secondary model/provider/contour or deterministic fallback>`;
- `AI_PROVIDER_DEBUG_BUDGET_MIN: <bounded minutes; default 10 during BUILD unless task/timebox justifies another value>`.

A valid live preflight must run from the **same execution environment and transport path as the application**, not only from curl or another project. It must verify at minimum:

1. runtime/client dependencies can initialize;
2. DNS/TLS/network route works;
3. the configured credential is accepted without printing/persisting it;
4. the selected model is currently available to the account/project;
5. a minimal real inference request succeeds;
6. required response mode (for example structured JSON/schema) works when the product depends on it.

For `STATIC_SINGLE_FILE` with direct browser AI/API use, the browser itself is the application runtime. The preflight must therefore use the **exact judged browser path** (direct file or chosen static origin) and additionally verify:

- provider/browser-use policy permits it;
- CORS/origin/preflight behavior succeeds;
- the operator session token is never persisted or exposed outside memory;
- the data-transfer policy permits the demo payload to leave the browser to that endpoint.

Server-side success does not prove browser viability. Browser CORS/origin failure is a concrete reason to promote delivery to `LOCAL_APPLICATION`; “AI needs a token” by itself is not.

VPN/proxy behavior is part of the runtime contract. Explicitly determine whether the application uses a system tunnel, environment proxy variables, an explicit proxy, or direct networking. Do not assume that because an AI provider works in another application it will work here: SDK options such as `trust_env`, proxy support, custom CA settings and optional SOCKS dependencies can change the effective route.

Configured endpoint/model/key presence is **not** evidence of working AI. A mock success is also not live-provider evidence.

### Failure/timebox rule

If the provider preflight fails, diagnose only within the recorded debug budget, then execute the contingency instead of consuming the hackathon on provider setup:

1. same provider with a verified current model/route;
2. for a browser candidate, classify CORS/origin/browser-policy failure separately from auth/model/network failure;
3. another already allowed and reachable provider/contour;
4. deterministic fallback when Must still remains satisfied;
5. `BLOCKED` only when AI is mandatory and no compliant path exists.

Do not switch to a backend merely to hide a credential until the credential mode and browser preflight have been evaluated. Promote to `LOCAL_APPLICATION` when a real server trust boundary is required or the browser path is forbidden/non-viable.

If AI is mandatory and no live path is verified, surface the blocker **before** building an AI-dependent happy path. If AI is optional, the demo must remain useful without the provider.

## Provider/contour defaults when allowed

### Personal Internet
Prefer a **currently verified** allowed OpenAI-compatible provider/profile selected during intake. Existing local configuration from another project may be reused as a hint, but endpoint/key/model must be preflighted from this project's actual runtime. Secret values are never committed or shown. External transfer of internal/confidential data requires explicit permission.

When the solution otherwise fits `STATIC_SINGLE_FILE`, evaluate `OPERATOR_SESSION_BYOK` before introducing Python/Node/backend dependencies solely for credential storage. Use it only if direct browser use is allowed and the exact browser preflight passes.

### CPS internal
Prefer the existing OpenAI-compatible internal endpoint when verified from the actual hackathon runtime. Architecture: `bounded AI function → thin adapter → existing endpoint`. Do not deploy model weights/GPU/LLM infrastructure unless Must requires it.

### Official override
Organizer-required provider/service wins.

Reuse from personal repositories is separate from provider permission and must follow official pre-existing-code rules. Reuse only the smallest proven boundary, not an entire unrelated application stack.

## Implementation contract for USE_AI

- bounded AI responsibility and non-AI responsibilities;
- provider/contour adapter boundary;
- explicit `AI_CREDENTIAL_MODE` and secret/config field names without values;
- for `OPERATOR_SESSION_BYOK`: memory-only lifecycle, no persistence/logging/URL exposure, masked settings UI, clear reconnect/re-entry behavior;
- early live provider preflight or explicit `NOT_RUN` reason + first-task readiness gate;
- for browser-direct AI: exact-browser CORS/origin/provider-policy preflight before substantial implementation;
- current model/provider selection, not a stale/deprecated identifier copied from old config;
- explicit network/proxy/tunnel semantics when external networking is involved;
- timeout/error/fallback behavior;
- output parsing/validation/grounding;
- representative evaluation scenarios;
- deterministic tests around parsing/validation/business behavior;
- ordinary unit tests do not require live external provider unless explicitly justified;
- live-provider checks are small, explicit smoke/evidence checks, not hidden inside ordinary test suites.
