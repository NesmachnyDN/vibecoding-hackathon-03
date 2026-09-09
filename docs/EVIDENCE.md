# Evidence

Keep only factual current implementation/verification facts. Planned capability is not evidence.

## Product

- Problem/user: по официальному заданию руководителю нужен быстрый обзор проектных рисков и приоритетных действий.
- Demo-critical flow: официально требуется показать запуск, данные, классификацию, панель, фильтрацию, критический проект, причины оценки, рекомендацию и опциональный AI при наличии доступа.
- Observed result: WORKING — `index.html` открыт напрямую через `file://`; демонабор, локальный CSV, совместные фильтры, сводка и карточка проекта работают без сборки и без сетевой зависимости.

## Capabilities

| Capability | Evidence | Status |
|---|---|---|
| Детерминированная оценка риска | Browser-direct проверка: баллы/уровни в порядке fixture — 46/high, 3/low, 100/critical, 22/medium, 58/critical, 0/low; ненулевые компоненты показаны в карточке | WORKING |
| Панель руководителя | Browser-direct проверка: KPI 6/3/2, распределение 2/1/1/2, top-3 — Платёжный шлюз, Система отчётности, Мобильный кабинет | WORKING |
| CSV upload + filters | Browser-direct проверка: UTF-8 BOM/CRLF/LF, `;` и `,`, quoted delimiter/newline/escaped quote; импорт 3 строк пересчитывает score/сводку/карточку, risk+stage дают точное пересечение, empty и restore-demo состояния согласованы | WORKING |
| Карточка + deterministic action | Выбор «Платёжный шлюз» показывает 58%, 12 дней, 5/2 риска, четыре составляющие балла, исходный комментарий и действие с владельцем/сроком | WORKING |
| Optional AI connection surface | Пустая конфигурация и безопасный недоступный HTTPS endpoint дают явный error/unavailable status, не меняя KPI/таблицу; live provider не проверялся | PARTIAL |
| Optional AI recommendation | Provider/browser readiness не запускался | NOT_STARTED |

Allowed status: `WORKING | PARTIAL | NOT_VERIFIED | NOT_STARTED`.

## Verification

| Check | Exact command/scenario | Result/ref |
|---|---|---|
| Happy path | `node /tmp/i002-browser-test.mjs` в Chrome 153: direct-open `file:///…/index.html`; baseline 6 строк и точные score/KPI/distribution/top/detail; валидные `;` и `,` CSV по 3 строки; AND/zero/clear filters; restore-demo | PASS |
| Negative/edge | Тот же browser-direct прогон: отсутствующий header, completion 101, risk `1.5`, файл 2 MiB + 1 byte; submit пустой AI-формы и недоступный `https://127.0.0.1:9/v1` | PASS — каждая CSV-ошибка контекстна и не меняет dataset/filters/selection/detail; AI показывает error, core остаётся 6 строк |
| Layout/keyboard | Chrome 153 screenshots и DevTools emulation 1366×768 и 1024×768; Tab: CSV → restore → risk → stage → endpoint → model → token → check → clear → project selector | PASS — horizontal overflow/clipping отсутствуют, все контролы доступны, focus outline 3px |
| Static checks | inline-script parse через `node -e`; `git diff --check`; `rg` по CSP, HTML sinks, persistence/logging и file decode APIs | PASS — синтаксис/diff чисты; CSP прежний; imported values используют text APIs; storage/logging отсутствуют |

## AI

- AI_USAGE_DECISION: USE_AI, optional only; Must remains deterministic.
- Use case/provider/contour: short management recommendation; operator-configurable OpenAI-compatible browser-direct path if verified.
- Validation/evaluation/fallback evidence: safe live credential/path не предоставлен, поэтому provider/browser success check NOT_RUN; empty и unreachable connection cases проверены, deterministic core не зависит от их результата.
- Observed AI result: NOT_RUN / no provider success claim.

## Delivery / CI

- DELIVERY_PROFILE: STATIC_SINGLE_FILE, runtime проверен без внешних зависимостей.
- Canonical artifact/start path: direct-open корневого `index.html`; проверен реальный `file://` URL в Chrome 152.
- Delivery lifecycle evidence: первый и повторный запуск не требуют install/build; reload возвращает встроенный deterministic fixture.
- CI_REQUIRED: NO
- CI_STATUS: N/A
- CI evidence/ref: N/A

## Security / robustness

- Relevant controls verified: CSV ограничен 2 MiB, декодируется strict UTF-8, полностью валидируется до атомарной замены состояния и выводится через safe text APIs; file content не сохраняется и не отправляется в сеть. Masked token после submit удаляется из input; `localStorage`/`sessionStorage` пусты, persistence/log API в runtime-коде не используются. Fetch использует HTTPS-only endpoint, `credentials: omit`, `no-store`, `no-referrer` и 8-секундный timeout.

## Repository sync

- Mode/primary/mirror: GITHUB_PRIMARY / `NesmachnyDN/vibecoding-hackathon-03` / no mirror.
- Relevant branch/main parity: реализация ведётся на `feat/i002-csv-filters` от синхронизированного `main` (`9483dd70d13f513028d8cece4988496fc19c6096`); итоговая push parity фиксируется в handoff задачи.

## Known limitations

- Optional AI provider/browser/CORS/network/model readiness остаётся NOT_RUN и не считается PASS.
- Optional AI-generated management recommendation не реализована и остаётся вне I002.
