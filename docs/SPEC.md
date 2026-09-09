# Product Specification

Status: `READY_FOR_IMPLEMENTATION`

## Task and user

- Official task: разработать небольшой прототип приложения «Контроль проектных рисков» для загрузки проектных данных, автоматической оценки состояния и управленческого представления результатов.
- Primary user: руководитель проекта / руководитель портфеля проектов.
- Problem: информация о сроках, рисках и комментариях разрознена; сложно быстро определить, какие проекты требуют внимания и какие действия приоритетны.
- Observable outcome: за несколько секунд пользователь видит состояние портфеля, наиболее проблемные проекты, объяснимую оценку риска и дальнейшие действия.

## Demo-critical happy path

1. Открыть приложение и демонстрационный набор данных.
2. Увидеть автоматическую классификацию, KPI портфеля и распределение по уровням риска.
3. Загрузить CSV и получить пересчитанное представление без перезапуска приложения.
4. Отфильтровать проблемные проекты по уровню риска и этапу.
5. Открыть критический проект и увидеть показатели, причины оценки, комментарий и детерминированную рекомендацию.
6. При доступном совместимом API — получить дополнительную ИИ-рекомендацию; при недоступности API основной поток остаётся рабочим и показывает понятный статус.

## Scope

### Must
- Демонстрационный набор данных и загрузка CSV заданной структуры.
- Табличный список проектов.
- Детерминированный интегральный уровень риска: низкий / средний / высокий / критический.
- Видимое и воспроизводимое объяснение причин оценки.
- Панель руководителя: всего проектов, высокий+критический риск, без просрочки, наиболее проблемные проекты, распределение по уровням риска.
- Фильтры минимум по уровню риска и этапу.
- Карточка проекта: показатели, причины, исходный комментарий, алгоритмическая рекомендация дальнейших действий.
- Современный аккуратный интерфейс для ноутбука и демонстрации жюри.
- Основная функциональность полностью работает без внешнего ИИ/API.
- README с воспроизводимым запуском на другом компьютере.

### Should
- Понятная обработка некорректного CSV/пустых данных без падения интерфейса.
- Стабильная сортировка наиболее проблемных проектов по интегральному баллу.
- Хорошая визуальная иерархия и явное выделение высоких/критических состояний.

### Could
- Дополнительная управленческая рекомендация через совместимый OpenAI-compatible API.
- Дополнительные синтетические тестовые строки, если они улучшают демонстрацию.

### Won't
- Серверная БД, многопользовательская авторизация, фоновые процессы, постоянное хранение данных.
- Backend только ради хранения API-токена.
- Docker/Compose без отдельной объективной необходимости.
- Обязательная зависимость MVP от внешнего ИИ.

## Risk scoring contract

`score = min(100, overdue + open + critical + completion)`:

- `overdue = min(30, overdue_days * 3)`;
- `open = min(15, open_risks * 3)`;
- `critical = min(50, critical_risks * 25)`;
- `completion = 10`, если completion `<50`; `5`, если `50–69`; иначе `0`.

Risk level:

- `LOW`: 0–9;
- `MEDIUM`: 10–24;
- `HIGH`: 25–49;
- `CRITICAL`: 50–100.

Каждый ненулевой компонент должен формировать понятную пользователю причину. Алгоритм не использует скрытых/случайных эвристик.

Expected fixture results for the six supplied projects:

| Проект | Балл | Уровень |
|---|---:|---|
| Мобильный кабинет | 46 | высокий |
| Электронный архив | 3 | низкий |
| Платёжный шлюз | 100 | критический |
| Личный кабинет сотрудника | 22 | средний |
| Система отчётности | 58 | критический |
| Каталог услуг | 0 | низкий |

## Acceptance criteria

| ID | Observable criterion | Verification |
|---|---|---|
| AC-001 | Демоданные загружаются сразу; корректный CSV той же схемы заменяет набор, а некорректный файл даёт понятную ошибку без потери рабочего состояния | Browser scenario with valid + invalid fixture |
| AC-002 | Для каждого проекта рассчитываются балл/уровень по зафиксированным правилам и показываются причины | Compare six supplied fixture values with scoring contract |
| AC-003 | Панель показывает total, high+critical, без просрочки, top проблемных и распределение уровней | Six-row fixture: total=6, high+critical=3, no-overdue=2, distribution 2/1/1/2 |
| AC-004 | Фильтры риска и этапа совместно ограничивают таблицу и связанные представления без перезагрузки | Browser filter scenarios |
| AC-005 | Карточка выбранного проекта показывает показатели, причины, комментарий и детерминированное действие | Select «Платёжный шлюз» and inspect detail |
| AC-006 | Без API-ключа/при ошибке API все Must-функции работают, а ИИ-секция сообщает о недоступности без fake success | Run without credentials / forced failed connection |
| AC-007 | При доступном совместимом API оператор может получить дополнительную краткую рекомендацию; токен не хранится в source/storage/URL/logs и теряется при reload/close | Live browser preflight when credential available + storage inspection |
| AC-008 | Основной экран понятен на ноутбуке, проблемные проекты визуально выделены, критические действия и состояния читаемы | Manual review at target laptop viewport |
| AC-009 | Чистый checkout запускается открытием `index.html` без установки project dependencies; повторный запуск не требует setup | Execute README first/subsequent-run paths |

## Official constraints

- Required/forbidden technologies: конкретный стек не задан; архитектуру и способ запуска разработчик выбирает самостоятельно.
- Submission/hard gates: исходный код; README; работающий демонстрационный сценарий; краткое описание архитектурного решения; описание алгоритма оценки; презентация для жюри.
- Data/security restrictions: внешние API-ключи/токены не хранятся в репозитории; отсутствие/ошибка внешнего API не препятствует основной демонстрации.
- Event deadline/duration evidence: UNKNOWN; абсолютное время не предоставлено и не является блокером.

## Deliverable language

- DELIVERABLE_LANGUAGE: RU
- LANGUAGE_DECISION_SOURCE: DEFAULT_RU
- LANGUAGE_EVIDENCE: в официальном задании нет требования английского языка.

## Execution environment preflight

| Factor | State / evidence |
|---|---|
| Browser/OS/runtime constraints | Must-path требует только современный desktop browser; конкретная event-машина не preflighted, non-blocking |
| Docker/Compose | N/A для выбранного STATIC_SINGLE_FILE; task не требует Docker |
| Internet/registries | N/A для Must; интернет нужен только опциональному AI |
| Official repository/CI capability | GitHub repository write access confirmed; project CI не требуется по принятому контракту |
| External/internal API reachability | NOT_RUN; опциональный AI |
| Direct browser API viability (provider policy/CORS/origin) | NOT_RUN; проверяется ранним bounded gate при наличии операторских параметров |
| Network route for external APIs (direct/TUN/env proxy/internal) | UNKNOWN; уточняется только для опционального AI |
| Proxy/VPN/runtime transport compatibility | UNKNOWN; Must не зависит от внешней сети |
| Data-transfer restrictions | В задании запрета не указано; для AI до отдельного разрешения использовать только синтетические/демо-данные |
| Required credential/service presence | AI credential NOT_PROVIDED / non-blocking |

ENVIRONMENT_BLOCKERS: none

## AI decision

- AI_USAGE_DECISION: USE_AI (optional enhancement; not a Must dependency)
- Deterministic baseline: прозрачный risk score + правила формирования дальнейших действий полностью закрывают обязательную функциональность.
- AI_USE_CASE: краткая управленческая формулировка по показателям выбранного проекта и его комментарию.
- AI_VALUE_OVER_DETERMINISTIC: более естественная суммаризация контекста для жюри/руководителя; не используется для расчёта уровня риска.
- AI_EXECUTION_CONTOUR: browser-direct OpenAI-compatible API when verified; otherwise deterministic fallback.
- AI_PROVIDER_PROFILE: operator-configurable compatible endpoint/model; provider/model не фиксируются без live verification.
- AI_REUSE_POLICY/SOURCE: no product-code reuse planned.
- AI_CREDENTIAL_MODE: OPERATOR_SESSION_BYOK
- AI_SECRET_HANDLING: masked input, page-memory only; no source/config/localStorage/IndexedDB/cookies/URL/logging/echo; re-entry after reload/close.
- AI_OUTPUT_VALIDATION: plain text only, bounded length, displayed as optional AI output; never executable; clear error/unavailable state.
- AI_EVALUATION/FALLBACK: representative synthetic projects; on any provider failure keep deterministic recommendation and show unavailable/degraded status.
- AI_PROVIDER_READINESS: NOT_RUN
- AI_BROWSER_DIRECT_READINESS: NOT_RUN
- AI_NETWORK_PATH: UNKNOWN
- AI_MODEL_STATUS: UNKNOWN
- AI_PREFLIGHT_EVIDENCE: live credential/path unavailable during intake; no PASS claim.
- AI_PROVIDER_CONTINGENCY: deterministic recommendation; do not promote to backend solely to rescue optional AI.
- AI_PROVIDER_DEBUG_BUDGET_MIN: 10

## Stack and delivery

- STACK_DECISION: self-contained vanilla HTML/CSS/JavaScript using browser File API and DOM; no framework/package/runtime dependency.
- EXECUTABLE_ARTIFACT: YES
- DELIVERY_PROFILE: STATIC_SINGLE_FILE
- DELIVERY_RATIONALE: complete Must scope (CSV parsing, deterministic scoring, dashboard, filtering, detail) has no real server/database/background responsibility and can run entirely in-browser. Optional AI is not allowed to force backend complexity.
- STATIC_BROWSER_API_EVIDENCE: NOT_RUN for optional AI only; Must browser functionality is network-independent.
- Simpler profile rejected because: N/A; simplest compliant profile selected.
- COMMAND_FACADE: DIRECT_OPEN
- Canonical build/open/start/stop/test: build=N/A; open=`index.html`; start=open in browser; stop=close tab; verification=browser scenarios from AC.
- Required config names/ports/state: none for Must; optional AI fields `endpoint`, `model`, `token`; no fixed port.

## Project CI

- CI_REQUIRED: NO
- CI_PLATFORM: N/A
- CI_CONFIG_PATH: N/A
- CI_REQUIRED_CHECKS: N/A
- Decision evidence/reason: official task does not require CI; chosen artifact has no build/package dependency and adding a CI toolchain solely for a single static file would increase complexity without material demo/reproducibility value. Verification remains explicit and local.

## UI contract

- UI_REQUIRED: YES
- Primary surface/user task: management dashboard for immediate portfolio triage, then project detail.
- Demo viewport/device: laptop, target 1366×768; must remain usable at ≥1024 px width.
- Visual direction/design system: restrained professional light dashboard; clear hierarchy; semantic risk colors; minimal card count; content-first table and detail panel.
- Required states/accessibility/responsive expectations: visible loading/not-applicable only when needed; empty/CSV-error/AI-unavailable states; keyboard reachable interactive controls; visible focus; readable labels/contrast; no horizontal breakage at target viewport.
- AI settings UX when applicable: compact settings/dialog with endpoint/model plus masked token; explicit «Проверить соединение» and status; no secret echo/persistence; explain re-entry after reload.

## Open blockers

- none. Optional AI provider/browser readiness is unresolved but explicitly non-blocking.
