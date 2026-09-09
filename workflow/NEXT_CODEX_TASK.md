# Next Codex Task

TASK_ID: I002
MODE: CONTINUE
QUALITY_GATE_PROFILE: ITERATION
BASE_BRANCH: main
TARGET_BRANCH: feat/i002-csv-filters
APPROVED_BRANCH: feat/i001-core-dashboard
SOURCE_REVIEW: I001_PASS
CONTEXT_MODE: MINIMAL

## Goal

Закрыть оставшийся обязательный пользовательский поток работы с данными: добавить загрузку собственного CSV, безопасную валидацию с сохранением последнего рабочего состояния, возврат к демонстрационным данным и совместные фильтры по уровню риска и этапу. Все связанные представления должны пересчитываться без перезагрузки страницы.

## Resolved contract snapshot

- Deliverable / delivery profile: сохранить `STATIC_SINGLE_FILE`; весь runtime остаётся в корневом `index.html` на vanilla HTML/CSS/JavaScript, без framework/package manager/backend/container.
- CI contract: `CI_REQUIRED:NO`; не создавать project CI или toolchain ради этого среза.
- AI contract: optional `USE_AI` остаётся неблокирующим; существующий `OPERATOR_SESSION_BYOK` connection surface и memory-only lifecycle сохранить без расширения. Live provider readiness в I002 не требуется, так как срез не AI-dependent. Не реализовывать AI-рекомендацию в этой задаче.
- UI contract: RU, laptop-first 1366×768, usable ≥1024; новые controls должны быть компактными, keyboard-reachable, с ясными success/error/empty состояниями и без перегрузки dashboard.
- Official hard gates relevant now: пользователь может использовать демоданные или загрузить CSV; фильтры минимум по уровню риска и этапу; invalid CSV не должен ломать рабочее состояние; core работает без внешнего API.

Скоринг и уровни риска не изменять:
`score = min(100, min(30, overdueDays*3) + min(15, openRisks*3) + min(50, criticalRisks*25) + completionPenalty)`;
`completionPenalty = 10` при `<50%`, `5` при `50–69%`, иначе `0`; уровни 0–9 low, 10–24 medium, 25–49 high, 50–100 critical.

CSV logical schema:
- `Проект`
- `Этап`
- `Выполнение, %`
- `Просрочка, дней`
- `Открытые риски`
- `Критические риски`
- `Комментарий`

CSV parser requirements:
- UTF-8 text; tolerate UTF-8 BOM and CRLF/LF;
- support comma and semicolon delimiters;
- correctly handle quoted fields, escaped quotes and delimiters/newlines inside quoted values;
- determine the delimiter by successful recognition of the required header set rather than naive character counting;
- required headers must exist exactly once; extra columns may be ignored;
- trim surrounding whitespace outside quoted content where safe;
- require non-empty project/stage; comment may be empty and then render a neutral «Комментарий не указан» value;
- numeric fields are integers; completion must be 0..100; overdue/open/critical risks must be >=0;
- require at least one valid data row; reject the whole import on any invalid row and report row/field context without replacing the currently working dataset.

State behavior:
- successful CSV import atomically replaces the active dataset, resets filters to «Все», rebuilds stage options and selects an appropriate first/top project;
- «Демонстрационные данные» restores the exact six-row fixture, clears import error/success state and resets filters;
- filtering is AND-combined: risk level + exact stage;
- table, KPI/distribution/top list and detail selection must stay consistent with the filtered set; if the selected project leaves the filtered set, select the highest-risk visible project (or first visible on tie); for an empty filtered set show a clear empty state and no stale detail from a hidden project;
- dataset/source indicator should make clear whether demo data or imported CSV is active and how many projects are in the active dataset/visible subset.

## Context pack

- `index.html`

Do not preload SPEC/PLAN/QUALITY/DELIVERY/AI/EVIDENCE/SUBMISSION; the execution-relevant contract is copied above.

## Owning context / invariants

- `index.html` remains the only runtime artifact and owns active dataset/filter/UI state.
- Existing pure risk scoring and deterministic recommendation logic remain the single source of truth for demo and imported rows.
- Import is atomic: parse/validate/evaluate into a candidate dataset first; commit to active UI state only after complete success.
- User file content is untrusted; insert values only via safe text APIs (`textContent`/created text nodes), never `innerHTML`.
- No persistence, database or browser storage is introduced.

## Expected reuse

- Reuse existing `calculateRisk`, `recommendedAction`, `portfolioFacts`, rendering and project-selection logic; refactor only as needed to support mutable active datasets and filters without duplicating business rules.

## Task-specific security / AI / delivery / CI / UI deltas

- File input accepts CSV/text files only; no filesystem paths, uploads to servers or network transfer.
- Bound the accepted file size to a reasonable demo-safe limit (for example 2 MiB) and surface a clear validation error when exceeded.
- Preserve CSP and the current AI token guarantees; CSV work must not broaden `connect-src` or introduce storage/logging of token/file contents.
- Do not start optional AI generation, Docker, backend or CI work.

## In scope

1. Add compact data controls: choose CSV file, import status/error, and explicit restore-demo action.
2. Implement robust dependency-free CSV parsing/validation per the contract above.
3. Replace the active dataset atomically on successful import and recompute scoring/dashboard/detail from the imported rows.
4. Add risk-level and stage filters with AND semantics; stage options derive from the active dataset.
5. Keep KPI/distribution/top/table/detail consistent with the filtered subset, including empty-result behavior.
6. Preserve and regression-check existing demo scoring, detail, AI-unavailable behavior, accessibility and direct-open delivery.
7. Update README only where the real demo/user flow changes; do not change the canonical direct-open launch model.
8. After verification, update only factual changed capability/verification evidence in `docs/EVIDENCE.md`.

## Out of scope

- Optional AI-generated management recommendation / live provider integration beyond the existing connection check.
- New scoring heuristics or changes to the six-row demo fixture.
- Backend, Python/Node runtime, database, Docker/Compose, package manager/framework.
- Project CI.
- Presentation/final submission/finalization work.
- Cosmetic redesign unrelated to the new controls/states.

## Acceptance criteria

- AC-001 → app starts on the exact six-row demo fixture; a valid CSV matching the logical schema replaces the dataset and all dependent views; invalid CSV gives a specific understandable error and leaves the previous dataset/results unchanged; restore-demo returns exactly to the original six projects.
- AC-004 → risk and stage filters combine with AND semantics and consistently update table, KPI/distribution/top list and detail selection without reload; zero-result filtering shows an explicit empty state without stale hidden-project detail.
- AC-002/AC-003 regression → restored demo fixture still produces scores 46/3/100/22/58/0 and portfolio facts 6 total, 3 high+critical, 2 no-overdue, distribution 2/1/1/2, top three Payment gateway / Reporting system / Mobile office in the established Russian names/order.
- AC-005 regression → after restore-demo, selecting «Платёжный шлюз» still shows 58%, 12 days, 5 open / 2 critical, all non-zero score reasons, original comment and deterministic action.
- AC-006 regression → CSV/filter errors and empty AI settings do not break deterministic core; no fake AI success.
- AC-008 → import/filter controls are readable and keyboard-usable at 1366×768 and ≥1024px, with visible focus and understandable error/empty/success feedback.
- AC-009 regression → clean checkout and subsequent run still require only direct opening of `index.html`; no install/build step added.

## Required checks

1. Start from refreshed `main` and verify it contains accepted I001 commit/content before creating `feat/i002-csv-filters`.
2. Direct-open committed `index.html` through the canonical `file://` path; confirm the restored/demo baseline still matches all six established scores and aggregate facts exactly.
3. Import a valid semicolon-delimited CSV with at least three rows, including one quoted comment containing a semicolon and one empty comment. Verify row count, scores/levels, source indicator and dashboard/detail recomputation.
4. Import a valid comma-delimited CSV whose two comma-containing header names are correctly quoted; verify the same parser path accepts it.
5. Verify atomic failure with at least: missing required header; completion outside 0..100; non-integer risk value; oversized file. After each failure confirm the previously active dataset, filters and selected/detail state remain usable and unchanged unless the contract explicitly resets only error text.
6. On an imported dataset containing at least one `Разработка` high/critical project and other stages/levels, apply risk + stage filters together and verify only the intersection remains; KPI/distribution/top/detail correspond to that visible subset.
7. Force a zero-result filter combination and verify table/summary/detail do not display stale data from a filtered-out project; then clear filters and verify full active dataset returns.
8. Restore demo data and verify exact original fixture, dynamic stage options, cleared filters and original score/portfolio regressions.
9. Regression-check optional AI with empty config and one safe unreachable endpoint: core remains usable and token lifecycle/storage behavior from I001 is unchanged.
10. Inspect untrusted CSV rendering path: no imported values reach `innerHTML`; no file content/token is persisted/logged; CSP and direct-open delivery remain intact.
11. Check layout at 1366×768 and 1024×768 plus keyboard tab order through data controls, filters, AI controls and project selectors; verify no demo-critical clipping.
12. Run `git diff --check` and any focused static/browser assertions introduced; do not add dependency/toolchain solely for tests.
13. Update `docs/EVIDENCE.md` only with actually observed results; unrun live AI remains NOT_RUN.
14. Commit to `feat/i002-csv-filters`, push to `origin`, and verify remote branch head SHA equals the completed local task commit SHA.

## Stop only when

- topology/history conflict cannot be safely reconciled;
- unexpected user changes would be overwritten;
- direct-file browser APIs needed for local CSV import are genuinely unavailable;
- unresolved data-integrity/security issue prevents atomic safe import;
- task conflicts with official rules or resolved contract.

Optional AI/provider unavailability is not a stop condition for I002.

## Notes

This is the remaining Must-completion slice. Keep the change focused on data import/filtering/error recovery and regressions; do not consume time on optional AI generation or final presentation work.
