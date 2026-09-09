# Evidence

Keep only factual current implementation/verification facts. Planned capability is not evidence.

## Product

- Problem/user: по официальному заданию руководителю нужен быстрый обзор проектных рисков и приоритетных действий.
- Demo-critical flow: запуск, демоданные, классификация, панель, локальный CSV, совместные фильтры, критический проект, причины оценки и детерминированная рекомендация.
- Observed result: WORKING — content baseline `f8645473c7c61b00e4ada0bcc9027d900a708db9` дважды открыт напрямую через `file://` из чистого detached worktree; полный поток работает без сборки, настройки, persistence и сети.

## Capabilities

| Capability | Evidence | Status |
|---|---|---|
| Детерминированная оценка риска | Browser-direct проверка: баллы/уровни в порядке fixture — 46/high, 3/low, 100/critical, 22/medium, 58/critical, 0/low; ненулевые компоненты показаны в карточке | WORKING |
| Панель руководителя | Browser-direct проверка: KPI 6/3/2, распределение 2/1/1/2, top-3 — Платёжный шлюз, Система отчётности, Мобильный кабинет | WORKING |
| CSV upload + filters | Chrome 153 browser-direct: `;` CSV с BOM/CRLF, quoted delimiter/newline/escaped quote/empty comment и `,` CSV с quoted comma-containing headers; импорт пересчитывает связанные views, risk+stage дают точное пересечение, zero/clear/restore согласованы | WORKING |
| Карточка + deterministic action | Выбор «Платёжный шлюз» показывает 58%, 12 дней, 5/2 риска, четыре составляющие балла, исходный комментарий и действие с владельцем/сроком | WORKING |
| Ошибки и восстановление | Missing header, completion >100, non-integer risk и файл 2 MiB + 1 byte дают понятные ошибки, не меняют последнее рабочее состояние; success/error/zero states проверены | WORKING |
| AI | Не входит в финальный scope; controls/runtime/provider claims удалены, детерминированная рекомендация сохранена | WORKING |

Allowed status: `WORKING | PARTIAL | NOT_VERIFIED | NOT_STARTED`.

## Verification

| Check | Exact command/scenario | Result/ref |
|---|---|---|
| README first run | В clean detached worktree `/tmp/i003-final-7VnwQu/repo` на baseline `f864547…`: `xdg-open index.html`, затем `node /tmp/i003-browser-test.mjs /tmp/i003-final-7VnwQu/repo/index.html` | PASS — команда открытия exit 0; Chrome 153 открыл точный `file://` artifact, полный browser flow PASS |
| README subsequent run | После завершения первого Chrome process повторно: `xdg-open index.html && node /tmp/i003-browser-test.mjs /tmp/i003-final-7VnwQu/repo/index.html` | PASS — новый browser process вернулся к исходному fixture и повторил полный flow без setup/network; checkout остался clean |
| Happy path | Оба Chrome 153 прогона: baseline 6 строк, scores `46/3/100/22/58/0`, KPI `6/3/2`, distribution `2/1/1/2`, установленный top-3; оба valid CSV; AND/zero/clear; restore-demo; полный detail «Платёжный шлюз» | PASS |
| Negative/edge | Те же прогоны: missing header, completion `101`, risk `1.5`, файл `2097153` bytes | PASS — каждая ошибка контекстна и не меняет dataset/filters/selection/detail |
| Layout/keyboard | Chrome 153 DevTools emulation и screenshots 1366×768/1024×768; Tab: CSV → restore → risk → stage → project; computed focus outline 3px | PASS — page overflow/control clipping отсутствуют; на 1024 таблица и карточка складываются вертикально, столбец риска виден |
| Static/security | inline-script parse через `node -e`; `rg` по network/persistence/logging/unsafe HTML sinks/AI controls; high-confidence secret scan; `git diff --check`; clean-tree/SHA assertions | PASS — CSP запрещает сеть по `default-src 'none'`; runtime requests/exceptions = 0; импорт выводится через text APIs; secret patterns не найдены |

## AI

- AI_USAGE_DECISION: NO_AI.
- Provider/credential/browser-direct readiness: N/A; runtime не содержит AI/provider integration, token handling или AI controls.
- Final baseline: только прозрачная детерминированная рекомендация; live AI success не заявляется.

## Delivery / CI

- DELIVERY_PROFILE: STATIC_SINGLE_FILE, runtime проверен без внешних зависимостей.
- Canonical artifact/start path: direct-open корневого `index.html`; проверен реальный `file://` URL в Google Chrome 153.0.8010.36.
- Delivery lifecycle evidence: README first-run и subsequent-run выполнены из clean committed checkout; оба запуска не требуют install/build/config/network и возвращают встроенный deterministic fixture.
- CI_REQUIRED: NO
- CI_STATUS: N/A
- CI evidence/ref: N/A

## Security / robustness

- Relevant controls verified: CSV ограничен 2 MiB, декодируется strict UTF-8, полностью валидируется до атомарной замены состояния и выводится через `textContent`/`createTextNode`; file content не сохраняется, не логируется и не отправляется в сеть. Runtime не использует network, persistence, cookies, unsafe HTML sinks или token handling; CSP не разрешает `connect-src`.

## Repository sync

- Mode/primary/mirror: GITHUB_PRIMARY / `NesmachnyDN/vibecoding-hackathon-03` / no mirror.
- Content baseline parity: после push/fetch `main == origin/main == f8645473c7c61b00e4ada0bcc9027d900a708db9`.
- Final process metadata parity: проверяется после metadata commit и фиксируется в I003 handoff, поскольку commit не может содержать собственный SHA.

## Known limitations

- Обязательная презентация ещё не создана; её генерация остаётся за финальным Chat review после product FINAL acceptance.
- Официальный submission mechanism не указан в доступном контракте.
