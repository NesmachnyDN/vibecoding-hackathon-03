# Next Codex Task

TASK_ID: I003
MODE: FINALIZE
QUALITY_GATE_PROFILE: FINAL
BASE_BRANCH: main
TARGET_BRANCH: main
APPROVED_BRANCH: feat/i002-csv-filters
SOURCE_REVIEW: I002_PASS
CONTEXT_MODE: FINALIZATION

## Goal

Стабилизировать и окончательно проверить интегрированное решение после закрытия Must scope, устранить только финальные несоответствия/неполные пользовательские поверхности, привести документацию и evidence к фактическому состоянию и подготовить репозиторий к финальному acceptance/submission без добавления новых функций.

## Resolved contract snapshot

- Final product scope: детерминированный `STATIC_SINGLE_FILE` в корневом `index.html`; demo data + local CSV import/validation + risk/stage filters + transparent scoring/dashboard/detail/recommendation полностью являются обязательным judged path.
- Delivery: direct-open `index.html`; без framework/package manager/backend/container и без install/build шага; чистый checkout и повторный запуск должны быть проверены точно по README.
- CI: `CI_REQUIRED:NO`, `CI_STATUS:N/A`; не добавлять CI/toolchain в FINALIZE.
- Language: RU для README, UI, submission и будущей презентации.
- AI final-scope decision: optional AI-generated recommendation не реализована, live provider/browser readiness остаётся `NOT_RUN`, а Must уже закрыт. Не начинать позднюю AI-функцию. Устранить из judged UI/README незавершённую AI connection/settings поверхность и привести SPEC/EVIDENCE к финальному `AI_USAGE_DECISION=NO_AI` с детерминированной рекомендацией как итоговым baseline. Не добавлять backend/provider code.
- Security: никакие секреты, токены, локальные файлы или их содержимое не должны попадать в Git/storage/logs/URL; CSV остаётся локальным untrusted input и выводится только safe text APIs.
- Submission hard gates: исходный код; воспроизводимый README; рабочий demo scenario; краткое описание архитектуры; описание алгоритма риска; презентация для жюри. Факты о презентации не выдумывать: фактическую deck generation выполняет финальный Chat review после product FINAL acceptance по контракту `docs/SUBMISSION.md`.

## Context pack

FINALIZE использует расширенный пакет:

- `index.html`
- `README.md`
- `docs/QUALITY.md`
- `docs/EVIDENCE.md`
- `docs/SUBMISSION.md`
- `docs/SPEC.md`
- `docs/PLAN.md`
- `docs/DELIVERY.md`
- `docs/AI.md`

## Owning context / invariants

- `index.html` остаётся единственным runtime artifact.
- Зафиксированный risk scoring contract и исходные шесть demo rows не менять.
- FINALIZE не добавляет функций; допустимы только устранение незавершённой optional-AI поверхности, required reliability/documentation/evidence/submission fixes и demo-critical исправления.
- Factual evidence only: не отмечать unrun check как PASS и не объявлять презентацию созданной до её реального появления.
- Product/content baseline должен быть закоммичен до записи его SHA в submission metadata.

## In scope

1. Убедиться, что текущий `main` содержит принятые I001+I002 и рабочий `index.html`.
2. Удалить незавершённую optional-AI connection/settings поверхность и связанный runtime-код/README текст, так как AI recommendation не реализована и late feature запрещена; сохранить детерминированную рекомендацию и core без сетевой зависимости.
3. Привести `docs/SPEC.md`, `docs/PLAN.md`, `docs/EVIDENCE.md` и README к фактическому финальному deterministic scope; `AI_USAGE_DECISION=NO_AI`, без ложных provider claims.
4. Выполнить полный FINAL gate из `docs/QUALITY.md` на интегрированном committed state.
5. Из чистого или эквивалентно изолированного checkout выполнить README first-run path ровно как написано; затем закрыть/повторно открыть приложение и выполнить subsequent-run path.
6. Прогнать полный demo-critical flow: demo baseline; scoring/KPI/distribution/top; valid CSV import; invalid CSV atomic recovery; AND filters + zero state; restore demo; critical project detail/reasons/comment/deterministic action.
7. Повторно проверить 1366×768 и 1024×768, keyboard/focus, error/empty/success states и отсутствие demo-critical clipping.
8. Проверить security/robustness: нет секретов/токенов; no persistence/logging/file upload/network transfer; imported CSV не достигает unsafe HTML sinks; CSP и dependency-free direct-open delivery остаются согласованными.
9. Выполнить `git diff --check` и релевантные существующие browser/static checks без добавления toolchain ради тестов.
10. Обновить `docs/EVIDENCE.md` точными final verification фактами и `docs/SUBMISSION.md` всеми подтверждёнными artifact/run/compliance фактами. Presentation оставить `NOT_CREATED/PENDING_CHAT_GENERATION` до фактической генерации, не выдумывать READY только ради неё.
11. Создать accepted content baseline commit на `main`; записать его SHA как `FINAL_COMMIT_SHA`/`FINAL_CONTENT_SHA` в submission metadata, затем отдельным metadata commit завершить process state. Любое последующее product behavior изменение инвалидирует baseline.
12. Push `main` в `origin` и подтвердить observed remote `main` SHA/parity.

## Out of scope

- Новая AI-рекомендация, live provider integration или provider debugging.
- Новые product features, новые scoring heuristics, дополнительные визуализации/analytics.
- Backend, Docker/Compose, package manager/framework, database.
- Project CI, coverage beautification или инфраструктурные улучшения.
- Cosmetic redesign, не связанный с финальной читаемостью/демо-дефектом.
- Выдумывание submission/presentation evidence.

## Acceptance criteria

- AC-001..AC-006, AC-008, AC-009 → все обязательные критерии из SPEC подтверждены integrated final browser evidence после I002.
- AC-007 → исключён из финального claimed scope как optional AI; `NO_AI` и deterministic fallback согласованы в SPEC/EVIDENCE/UI/README, отсутствуют незавершённые или вводящие в заблуждение AI controls.
- FINAL-DELIVERY → README first-run и subsequent-run работают из committed state без install/build/hidden shell state.
- FINAL-SECURITY → repository/browser inspection не выявляет секретов, persistence, unsafe CSV rendering или скрытой сетевой зависимости core.
- FINAL-UI → demo-critical flow читаем и управляем с клавиатуры на 1366×768 и 1024×768.
- FINAL-SUBMISSION → README содержит запуск, архитектурное решение, алгоритм и демонстрационный сценарий; SUBMISSION содержит только фактические финальные refs/statuses и явно показывает, что обязательная презентация генерируется после final Chat acceptance.
- FINAL-SYNC → final metadata commit опубликован в `origin/main`, remote SHA совпадает с локальным final process commit.

## Required checks

1. Pre-run freshness gate: `EXPECTED_TASK_ID=I003 == STATE.CURRENT_TASK_ID == NEXT.TASK_ID`, local `main` fast-forwarded to `origin/main`, worktree clean до product edits.
2. Подтвердить, что accepted I002 commit `0d7a72d653e86c9c847a9c366ea3185de2725d50` является предком текущего `main` и I002 runtime присутствует.
3. После удаления incomplete AI surface убедиться, что в UI/README нет кнопок/статусов, обещающих AI capability; core не содержит provider fetch/token handling, если оно больше не нужно.
4. Из clean/equivalent checkout выполнить README first-run direct-open path; проверить initial demo fixture: scores `46/3/100/22/58/0`, total=6, high+critical=3, no-overdue=2, distribution `2/1/1/2`, top-3 в установленном порядке.
5. Проверить valid `;` CSV с quoted delimiter/newline/empty comment и valid comma CSV с quoted comma-containing headers; оба импорта пересчитывают связанные views.
6. Проверить atomic failures: missing header, completion >100, non-integer risk, oversized file; последнее рабочее состояние не меняется.
7. Проверить AND risk+stage filter, zero-result state, clear/restore-demo и отсутствие stale detail.
8. После restore-demo выбрать «Платёжный шлюз» и проверить metrics, все ненулевые reasons, исходный comment и deterministic action.
9. Выполнить subsequent-run path после закрытия/повторного открытия; состояние возвращается к deterministic demo baseline без setup/network dependency.
10. Проверить layout/keyboard/focus при 1366×768 и 1024×768 и основные error/empty/success states.
11. Выполнить secret/storage/network/unsafe-sink inspection и `git diff --check`; использовать только существующие lightweight browser/static проверки.
12. Обновить factual EVIDENCE/SPEC/PLAN/README и submission facts, не помечая presentation как существующую.
13. Создать content baseline commit, затем metadata commit с `FINAL_COMMIT_SHA`/`FINAL_CONTENT_SHA`; push `main` и проверить remote SHA parity.
14. Если любой mandatory FINAL gate не проходит — оставить submission `NOT_READY`/`BLOCKED`, явно записать blocker и не маскировать его статусом READY.

## Stop only when

- интегрированное Must behavior не удаётся стабилизировать без изменения официального контракта;
- обнаружена security/data-integrity проблема, требующая отдельного архитектурного решения;
- clean-checkout judged path не воспроизводится;
- repository history/sync нельзя безопасно завершить без rewriting;
- официальный hard gate не может быть удовлетворён в текущем контуре.

## Notes

Это FEATURE-FREEZE/FINALIZE работа. Не добавляй optional AI или новые judged features. После успешного push остановись; финальный Chat review применит FINAL gate по фактам и при READY сгенерирует обязательную презентацию по `docs/SUBMISSION.md`.
