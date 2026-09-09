# Next Codex Task

TASK_ID: I001
MODE: IMPLEMENT
QUALITY_GATE_PROFILE: ITERATION
BASE_BRANCH: main
TARGET_BRANCH: feat/i001-core-dashboard
APPROVED_BRANCH: NONE
SOURCE_REVIEW: INTAKE
CONTEXT_MODE: MINIMAL

## Goal

Создать первый работающий вертикальный срез «Контроль проектных рисков»: один self-contained `index.html`, который на демонстрационных данных рассчитывает объяснимый риск, даёт руководителю портфельный обзор и детальную карточку проекта. Одновременно создать безопасную optional-AI connection surface/readiness behavior, не делая AI зависимостью core.

## Resolved contract snapshot

- Deliverable / delivery profile: `STATIC_SINGLE_FILE`; vanilla HTML/CSS/JavaScript; без framework/package manager/backend/container; canonical Must launch = direct-open `index.html`.
- CI contract: `CI_REQUIRED:NO`; не создавать `.github/workflows`, package/toolchain только ради CI или build pipeline.
- AI contract: `USE_AI` только как optional enhancement; core полностью deterministic. `OPERATOR_SESSION_BYOK`; endpoint/model/token вводит оператор, token только page-memory, masked, без source/config/localStorage/IndexedDB/cookies/URL/logs/echo. `AI_PROVIDER_READINESS=NOT_RUN`, `AI_BROWSER_DIRECT_READINESS=NOT_RUN`, debug budget 10 мин. Если live credential/path недоступен — зафиксировать NOT_RUN и продолжить core. Если browser/API check FAIL — показать понятный статус и оставить deterministic fallback; не добавлять backend только ради optional AI.
- UI contract: профессиональный светлый management dashboard, content-first, laptop 1366×768 (usable ≥1024); semantic risk colors; явные focus/error/AI-status states; без card soup и декоративного filler.
- Language / official hard gate relevant to this task: RU. Core обязан работать без внешнего API. Никаких реальных секретов в Git. Итоговая сдача позднее требует README, architecture description, risk algorithm and presentation.

Risk algorithm for this task:

`score = min(100, min(30, overdueDays*3) + min(15, openRisks*3) + min(50, criticalRisks*25) + completionPenalty)`;
`completionPenalty = 10` if `<50%`, `5` if `50–69%`, else `0`.
Levels: 0–9 low; 10–24 medium; 25–49 high; 50–100 critical.
Every non-zero component must appear as a human-readable reason.

Required demo fixture (do not silently change values):

| Project | Stage | Completion | Overdue | Open risks | Critical risks | Comment |
|---|---|---:|---:|---:|---:|---|
| Мобильный кабинет | Разработка | 72 | 4 | 3 | 1 | Не согласован новый срок интеграционного тестирования |
| Электронный архив | Тестирование | 91 | 0 | 1 | 0 | Испытания идут по плану |
| Платёжный шлюз | Разработка | 58 | 12 | 5 | 2 | Не получен доступ к внешнему тестовому контуру |
| Личный кабинет сотрудника | Проектирование | 45 | 2 | 2 | 0 | Требуется уточнить требования подразделения кадров |
| Система отчётности | Внедрение | 96 | 7 | 4 | 1 | Обнаружена проблема производительности при формировании отчётов |
| Каталог услуг | Разработка | 81 | 0 | 0 | 0 | Работы выполняются в соответствии с планом |

Expected scores/levels: 46/high; 3/low; 100/critical; 22/medium; 58/critical; 0/low in the same row order.
Expected portfolio facts: total=6; high+critical=3; no-overdue=2; distribution low=2, medium=1, high=1, critical=2; top three by score = Платёжный шлюз, Система отчётности, Мобильный кабинет.

## Context pack

- NONE

Task contract above intentionally contains the needed greenfield facts. Do not preload all process docs.

## Owning context / invariants

- `index.html` owns the entire runtime for this slice; keep CSS/JS embedded and logically sectioned rather than adding runtime files/dependencies.
- Risk scoring/reasons/recommendation must be deterministic pure logic, separate from DOM rendering even inside the single file.
- AI output never influences risk score/level.
- Demo data is synthetic and embedded; no persistence required.
- Keep core usable with network disabled and without AI credentials.

## Expected reuse

- none; greenfield product implementation.

## Task-specific security / AI / delivery / CI / UI deltas

- At the start, determine whether a safe live operator credential/path is actually available without searching for or printing secrets. If unavailable, record AI readiness `NOT_RUN` and proceed; do not spend time trying to discover credentials.
- If available, use the exact browser/open mode intended for the demo to test endpoint/model/token acceptance and CORS/origin within 10 minutes. Classify provider/browser failure separately from core product status.
- Implement a compact AI settings/connection area with endpoint, model and masked token plus explicit «Проверить соединение» status. A failed/empty AI config must never look like success.
- Do not yet implement the final AI management recommendation generation beyond what is strictly needed for connection/readiness surface; that belongs to a later slice after review.

## In scope

1. Create self-contained `index.html` with embedded CSS/JS and the exact six-row demo fixture.
2. Implement the scoring contract, risk labels/colors and explicit reason list.
3. Render management KPIs: total, high+critical, no-overdue; render risk distribution and top three problematic projects.
4. Render project table with visually clear risk status; selecting a project shows all metrics, reasons, source comment and a deterministic recommended next action derived from its metrics.
5. Implement professional laptop-first layout and meaningful empty/error/disabled/focus states needed by this slice.
6. Implement optional-AI settings/connection readiness surface as described; keep token memory-only and core fully independent.
7. Update README only as needed so the actual first-run and subsequent-run instructions exactly match the implemented direct-open path.
8. After verification, update only factual changed capability/readiness/delivery evidence in `docs/EVIDENCE.md`.

## Out of scope

- CSV file upload/parsing and schema-error recovery (S2).
- Risk/stage filtering controls (S2).
- Final AI-generated project recommendation (S3, only after provider/readiness outcome).
- Backend, Python/Node server, database, Docker/Compose, package manager/framework.
- Project CI.
- Presentation/final submission work.
- Unrelated refactor/process-document rewrite.

## Acceptance criteria

- AC-002 → exact six fixture scores/levels match: 46/high, 3/low, 100/critical, 22/medium, 58/critical, 0/low; UI exposes score reasons.
- AC-003 → dashboard on fixture shows total=6, high+critical=3, no-overdue=2, distribution 2/1/1/2 and top three in required order.
- AC-005 → selecting «Платёжный шлюз» shows its metrics, score reasons, original comment and a deterministic actionable recommendation.
- AC-006(partial) → with empty/failed AI configuration the dashboard/detail remain fully usable and AI area shows a clear unavailable/not-configured/error state without fake success.
- AC-008(partial) → main flow is visually coherent and readable at 1366×768 with semantic distinction of high/critical projects and usable focus states.
- AC-009 → clean committed state requires no project dependency installation/build; direct opening of `index.html` is the documented and verified canonical path.

## Required checks

1. Before AI-dependent work, apply the bounded readiness gate above. Do not claim PASS without an actual live browser request. If no credential/path is available, record `NOT_RUN` and continue deterministic implementation.
2. Open committed `index.html` from the repository root using the canonical direct-file browser path; verify no uncaught console error on initial load.
3. Verify all six fixture scores/levels and the aggregate portfolio facts listed in this task exactly.
4. Select «Платёжный шлюз» and verify metric values, reason list, original comment and deterministic action are visible.
5. Verify the core still works with AI settings empty; trigger one safe invalid/unreachable AI connection case and confirm a clear error/unavailable state with no core regression.
6. Inspect runtime code/browser storage behavior: no token in source, DOM echo, URL, logs, `localStorage`, IndexedDB or cookies; reload/close loses entered token.
7. Check layout manually at 1366×768 (and at least 1024px width) for clipping/critical control accessibility; keyboard-tab through primary controls.
8. Run `git diff --check` and any focused static/browser checks introduced by the implementation; do not add a dependency/toolchain solely to satisfy this line.
9. Update `docs/EVIDENCE.md` only with observed results; unrun live AI remains NOT_RUN.
10. Commit to `feat/i001-core-dashboard`, push to `origin`, and verify pushed branch SHA equals the local task commit SHA.

## Stop only when

- topology/history conflict cannot be safely reconciled;
- unexpected user changes would be overwritten;
- unresolved ownership/data-integrity/security decision blocks the active contract;
- direct browser execution of the deterministic Must slice is genuinely unavailable;
- task conflicts with official rules or resolved contract.

Optional AI/provider unavailability is not a stop condition for I001.

## Notes

Use the compact ITERATION baseline from AGENTS plus Required checks. Do not expand into CSV/filter/AI-generation/finalization scope.
