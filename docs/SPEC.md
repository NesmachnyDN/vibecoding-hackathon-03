# Product Specification

Status: `FINAL_CANDIDATE`

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
| AC-006 | Основной поток полностью работает без API, внешней сети и скрытого AI fallback | Browser scenario + static network inspection |
| AC-007 | Исключён из финального claimed scope: опциональная AI-функция не реализована, а незавершённые controls отсутствуют | UI/source/README inspection |
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
| Internet/registries | N/A; приложение не использует внешнюю сеть |
| Official repository/CI capability | GitHub repository write access confirmed; project CI не требуется по принятому контракту |
| External/internal API reachability | N/A; внешние API не используются |
| Direct browser API viability (provider policy/CORS/origin) | N/A |
| Network route for external APIs (direct/TUN/env proxy/internal) | N/A |
| Proxy/VPN/runtime transport compatibility | N/A |
| Data-transfer restrictions | CSV и его содержимое остаются локально в браузере |
| Required credential/service presence | N/A |

ENVIRONMENT_BLOCKERS: none

## AI decision

- AI_USAGE_DECISION: NO_AI
- Deterministic baseline: прозрачный risk score + правила формирования дальнейших действий полностью закрывают обязательную функциональность.
- AI_USE_CASE: N/A; опциональная функция не реализована до feature freeze.
- AI_VALUE_OVER_DETERMINISTIC: N/A; детерминированная рекомендация достаточна для итогового scope.
- AI_EXECUTION_CONTOUR: N/A.
- AI_PROVIDER_PROFILE: N/A.
- AI_REUSE_POLICY/SOURCE: no product-code reuse planned.
- AI_CREDENTIAL_MODE: N/A
- AI_SECRET_HANDLING: N/A; runtime не принимает и не обрабатывает токены.
- AI_OUTPUT_VALIDATION: N/A.
- AI_EVALUATION/FALLBACK: N/A; пользователь получает только детерминированную рекомендацию.
- AI_PROVIDER_READINESS: N/A
- AI_BROWSER_DIRECT_READINESS: N/A
- AI_NETWORK_PATH: N/A
- AI_MODEL_STATUS: N/A
- AI_PREFLIGHT_EVIDENCE: N/A; AI не входит в финальный продукт.
- AI_PROVIDER_CONTINGENCY: N/A.
- AI_PROVIDER_DEBUG_BUDGET_MIN: N/A

## Stack and delivery

- STACK_DECISION: self-contained vanilla HTML/CSS/JavaScript using browser File API and DOM; no framework/package/runtime dependency.
- EXECUTABLE_ARTIFACT: YES
- DELIVERY_PROFILE: STATIC_SINGLE_FILE
- DELIVERY_RATIONALE: complete scope (CSV parsing, deterministic scoring, dashboard, filtering, detail) has no server/database/background responsibility and runs entirely in-browser.
- STATIC_BROWSER_API_EVIDENCE: N/A; внешние API не используются.
- Simpler profile rejected because: N/A; simplest compliant profile selected.
- COMMAND_FACADE: DIRECT_OPEN
- Canonical build/open/start/stop/test: build=N/A; open=`index.html`; start=open in browser; stop=close tab; verification=browser scenarios from AC.
- Required config names/ports/state: none.

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
- Required states/accessibility/responsive expectations: visible empty/CSV-error/success states; keyboard reachable interactive controls; visible focus; readable labels/contrast; no horizontal breakage at target viewport.
- AI settings UX when applicable: N/A; AI controls отсутствуют.

## Open blockers

- none.
