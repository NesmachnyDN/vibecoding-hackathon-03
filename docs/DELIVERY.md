# Delivery, Reproducibility & Project CI

Choose the least complex compliant runtime.

Preference at equal suitability:

`STATIC_SINGLE_FILE → LOCAL_APPLICATION → CONTAINERIZED`

Official task/platform requirements override this order.

## Profiles

### STATIC_SINGLE_FILE
Use when the complete Must MVP runs as one self-contained browser artifact with no real server/database/worker responsibility.

AI/external API use does **not** by itself require a backend. A browser-only artifact may use an operator-entered session credential (`OPERATOR_SESSION_BYOK`) when all of the following are true:

- official rules, data policy and provider/browser terms allow direct browser use;
- the credential is supplied intentionally by the demo operator, is not embedded in source/build/config and is not a shared application secret;
- it remains memory-only for the current page/session: no `localStorage`, IndexedDB, cookies, URL/query string, analytics, logs, screenshots or persistence;
- the exact browser delivery path passes a live preflight for CORS/origin/network/model/required response features;
- loss of the credential on reload/close is acceptable and documented as prototype/demo behavior.

Prefer direct open. A tiny dependency-free static server is allowed only for browser restrictions when it remains simpler and more reproducible than an application backend. If the static server becomes a material runtime/setup dependency, reassess `LOCAL_APPLICATION`.

`OPERATOR_SESSION_BYOK` is a hackathon/demo trust model, not a claim that browser-held API credentials are suitable for production. Record the production hardening direction when relevant: server-side gateway, corporate API gateway, short-lived delegated token or another approved mechanism.

### LOCAL_APPLICATION
Use when a real local runtime/server/CLI is needed but Docker does not materially reduce setup/reproducibility risk. Examples include a long-lived/shared server secret, provider/browser policy that forbids direct client access, failed CORS/browser preflight, server-side authorization, file processing that must not run in-browser, database/state ownership, or another genuine server responsibility.

Do **not** select `LOCAL_APPLICATION` solely to read an AI/API token from an environment variable when an allowed session-only operator credential would satisfy the demo safely enough.

Keep one obvious start/test path.

### CONTAINERIZED
Use when explicitly required or materially justified by multiple services, native/system dependencies, fragile host setup, isolation or reproducibility. Normally expose root `make build`, `make up`, `make down`; add logs/test/reset/help only when useful. Compose only when orchestration actually needs it.

No secrets in image layers/build args/committed config; avoid privileged containers/socket/broad mounts; explicit runtime/base versions; non-root/readiness when practical.

### TASK_OVERRIDE / N/A
Use only for an evidenced official alternative or no executable artifact.

## Selection algorithm

1. Official Docker/alternative mechanism requirement wins.
2. No executable artifact → N/A.
3. Determine whether the Must scope has any **real server responsibility** independent of credential storage: server-side authorization, shared/persistent secret, database/state, non-browser processing, protected integration boundary, background work, unsupported browser protocol/provider policy, or equivalent.
4. If no real server responsibility exists, test the simplest browser path before introducing a runtime/framework:
   - no credential required → `STATIC_SINGLE_FILE` when the complete Must scope fits the browser;
   - external API/AI with allowed `OPERATOR_SESSION_BYOK` → preflight the **exact browser path** for CORS/origin/network/credential/model/required response mode without persisting or exposing the credential;
   - browser preflight PASS and Must fits → `STATIC_SINGLE_FILE`;
   - browser preflight FAIL/forbidden, or credential must be shared/long-lived/hidden from the operator → continue to step 5.
5. Runtime/backend genuinely required but containers add no material value → `LOCAL_APPLICATION`.
6. Multiple services/system dependencies/fragile host setup materially justify containers and Docker is permitted → `CONTAINERIZED`.

### Anti-overengineering rule

A secret-looking field is not automatically a server requirement. Distinguish:

- **application/shared secret** — must not be exposed to browser users; normally requires a server/approved gateway;
- **operator session BYOK** — the operator knowingly supplies their own temporary credential for this local demo session; may remain browser-only when the above safeguards and browser preflight pass;
- **official browser auth** — use the organizer/provider-approved browser mechanism when supplied.

When `STATIC_SINGLE_FILE` is rejected, `DELIVERY_RATIONALE` must name the concrete failed/forbidden browser condition or genuine server responsibility. “AI needs a token” is not sufficient rationale by itself.

## Reproducibility

- README has one canonical judged/demo path.
- For every executable artifact, README must distinguish **first run from a clean checkout** from **subsequent run** when setup is required.
- The canonical commands must be runnable from the repository root without hidden shell state: no dependence on a previously activated virtual environment, globally installed application packages/CLIs, IDE launch configuration, machine-specific aliases or uncommitted files.
- Project-managed dependencies must be installed before launch and the launch command must address the project-managed runtime/tool explicitly, or use a checked-in wrapper/Make target that does so.
- Platform prerequisites such as the required language runtime, Docker or browser may be documented explicitly; application dependencies must not be assumed globally installed.
- For Python `LOCAL_APPLICATION` on Linux, prefer an explicit project virtual environment path, for example `python3 -m venv .venv`, then `.venv/bin/python -m pip install ...`, then `.venv/bin/python -m <server-or-module> ...`. Do not make bare `python`, `pip`, `uvicorn`, `pytest` or similar globally resolved commands the only canonical path.
- If activation is shown as a convenience, it is secondary only; the canonical path must remain valid without `source .venv/bin/activate`.
- Dependency lockfile when dependencies exist.
- Required config names documented without secret values.
- For `OPERATOR_SESSION_BYOK`, README documents only the **operator action** and non-secret field names; it never contains a real token. Reload/close behavior must be explicit.
- Synthetic/demo state is deterministic enough for repeat runs.
- Final verification must execute the README first-run/install/start sequence as written from a clean or equivalently isolated committed checkout on the declared demo platform, then execute the subsequent-run path after restart. Record exact commands/results in EVIDENCE.
- No hidden machine-specific absolute paths/uncommitted files.

## External API / AI network reproducibility

When the judged path depends on an external/internal API, treat network routing as part of delivery rather than incidental workstation state.

- Record the intended route class: `DIRECT`, `SYSTEM_TUNNEL`, `ENV_PROXY`, `EXPLICIT_PROXY`, `INTERNAL` or `N/A`.
- Verify from the same runtime/client used by the application. For `STATIC_SINGLE_FILE`, that runtime **is the target browser and exact open/origin mode**; curl/Python/another application success does not prove browser CORS/origin compatibility. For `LOCAL_APPLICATION`, browser/curl success does not prove the project runtime route.
- Make proxy behavior intentional: know whether the SDK/browser honors environment proxy variables, uses the system route, requires an explicit proxy, or intentionally bypasses them.
- If SOCKS/HTTP proxy support needs an optional dependency, include it only when the selected demo route needs it and verify installation early.
- Avoid committed machine-specific proxy addresses. Local proxy/VPN values remain runtime/operator configuration.
- For direct browser API use, verify CORS/origin behavior and provider/browser-use policy before substantial implementation. A provider that works server-side may still be unusable from a static browser artifact.
- For AI providers, preflight the current model plus a minimal inference call before substantial AI-dependent implementation; see `docs/AI.md`.
- README may document alternative route instructions (for example system tunnel vs environment proxy) only when actually verified. One canonical judged route must remain obvious.

## Project CI

Initial repository contains no project CI.

Resolve during intake:

- `CI_REQUIRED:YES`: official platform supports useful safe automation; first runnable slice creates the minimal verification-only workflow.
- `NO`: unavailable/prohibited/unsafe/not useful; record reason.
- `N/A`: no meaningful build/test implementation path.

Official primary selects platform. A GitHub control-plane mirror never changes GitLab/V-Works CI ownership.

Minimal CI may restore dependencies, lint/typecheck, run targeted tests and build the actual artifact. Build Docker image only for CONTAINERIZED. No default deploy/publish/provision/external writes/secrets.

CI status semantics: `NOT_CONFIGURED | CONFIGURED | PASS | FAIL | UNAVAILABLE | N/A`. Config presence is never PASS.
