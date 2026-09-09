# Evidence

Keep only factual current implementation/verification facts. Planned capability is not evidence.

## Product

- Problem/user: по официальному заданию руководителю нужен быстрый обзор проектных рисков и приоритетных действий.
- Demo-critical flow: официально требуется показать запуск, данные, классификацию, панель, фильтрацию, критический проект, причины оценки, рекомендацию и опциональный AI при наличии доступа.
- Observed result: WORKING — `index.html` открыт напрямую через `file://`; шесть встроенных проектов, сводка и карточка проекта отрисованы без ошибки и без сетевой зависимости.

## Capabilities

| Capability | Evidence | Status |
|---|---|---|
| Детерминированная оценка риска | Browser-direct проверка: баллы/уровни в порядке fixture — 46/high, 3/low, 100/critical, 22/medium, 58/critical, 0/low; ненулевые компоненты показаны в карточке | WORKING |
| Панель руководителя | Browser-direct проверка: KPI 6/3/2, распределение 2/1/1/2, top-3 — Платёжный шлюз, Система отчётности, Мобильный кабинет | WORKING |
| CSV upload + filters | План/AC зафиксированы, runtime отсутствует | NOT_STARTED |
| Карточка + deterministic action | Выбор «Платёжный шлюз» показывает 58%, 12 дней, 5/2 риска, четыре составляющие балла, исходный комментарий и действие с владельцем/сроком | WORKING |
| Optional AI connection surface | Пустая конфигурация и безопасный недоступный HTTPS endpoint дают явный error/unavailable status, не меняя KPI/таблицу; live provider не проверялся | PARTIAL |
| Optional AI recommendation | Provider/browser readiness не запускался | NOT_STARTED |

Allowed status: `WORKING | PARTIAL | NOT_VERIFIED | NOT_STARTED`.

## Verification

| Check | Exact command/scenario | Result/ref |
|---|---|---|
| Happy path | Chrome 152 direct-open `file:///…/index.html`; DevTools Runtime assertions для 6 строк, score/level, KPI/distribution/top-3 и карточки «Платёжный шлюз» | PASS |
| Negative/edge | В direct-file сессии: submit пустой AI-формы; затем недоступный `https://127.0.0.1:1/v1`; reload после ввода токена | PASS — явные `Не настроено`/`Соединение недоступно`, core 6/3/2 и 6 строк сохранён, token field очищен, после reload token отсутствует |
| Layout/keyboard | Chrome screenshots и DevTools emulation 1366×768 и 1024×768; Tab через endpoint → model → token → check → project selectors | PASS — horizontal overflow отсутствует, активные контролы доступны, focus outline 3px |
| Static checks | `git diff --check`; `rg` по persistence/log/dummy-token API; браузерный reload с Runtime/Log enabled | PASS — ошибок diff/uncaught console нет, запрещённое хранение/логирование не найдено |

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

- Relevant controls verified: masked token после submit удаляется из input и не попадает в body text/URL; `localStorage`/`sessionStorage` пусты, cookie отсутствует, persistence/log API в runtime-коде не используются; reload очищает session token. Fetch использует HTTPS-only endpoint, `credentials: omit`, `no-store`, `no-referrer` и 8-секундный timeout.

## Repository sync

- Mode/primary/mirror: GITHUB_PRIMARY / `NesmachnyDN/vibecoding-hackathon-03` / no mirror.
- Relevant branch/main parity: реализация ведётся на `feat/i001-core-dashboard` от синхронизированного `main` (`055d3f8432afa07b53bbf878f3c83336c208d9f2`); итоговая push parity фиксируется в handoff задачи.

## Known limitations

- CSV upload/filtering и финальная AI-рекомендация не входят в I001 и пока не реализованы.
- Optional AI provider/browser/CORS/network/model readiness остаётся NOT_RUN и не считается PASS.
