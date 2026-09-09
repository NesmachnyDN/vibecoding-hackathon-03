# Контроль проектных рисков

Рабочий репозиторий проекта хакатона.

## Статус

Текущий workflow/status см. в `workflow/STATE.md`.

## Цель

TODO после intake: одно предложение о проблеме и наблюдаемом результате.

## Язык материалов

До intake базовый язык — русский. Intake фиксирует `DELIVERABLE_LANGUAGE` в `docs/SPEC.md`: явное официальное требование английского имеет приоритет, иначе `RU`.

## Запуск

TODO после intake/первого runnable slice. Используется самый простой профиль, закрывающий Must-сценарий:

`STATIC_SINGLE_FILE → LOCAL_APPLICATION → CONTAINERIZED`, если официальный task не требует другого.

Для исполняемого проекта README после первого runnable slice должен содержать два явно разделённых сценария:

1. **Первый запуск из чистого checkout** — все необходимые команды установки проектных зависимостей и запуска из корня репозитория.
2. **Последующие запуски** — минимальная команда запуска без повторной установки зависимостей.

Канонический путь не должен зависеть от скрытого состояния shell, ранее активированного виртуального окружения, глобально установленных application package/CLI, IDE-конфигурации, пользовательских alias или незакоммиченных файлов. Если используется Python virtualenv, основной Linux-пример должен вызывать `.venv/bin/python` напрямую; `source .venv/bin/activate` допустим только как дополнительное удобство, а не как обязательное скрытое условие.

## Рабочий цикл

`Intake → Codex → Review → Fix/Continue → Finalize → Submission`

- Intake: `prompts/CHAT-00-INTAKE.md`
- Codex: `prompts/CODEX-RUN.md`
- Review/recovery/final acceptance: `prompts/CHAT-10-REVIEW.md`

## Основные источники истины

- `workflow/STATE.md` — topology, phase/timebox, active task status.
- `workflow/NEXT_CODEX_TASK.md` — текущий bounded execution contract.
- `docs/SPEC.md` — acceptance, environment, AI/stack/delivery/CI/UI decisions.
- `docs/PLAN.md` — slices и phase strategy.
- `docs/QUALITY.md` — architecture/code/security/UI + ITERATION/FINAL gates.
- `docs/DELIVERY.md` — delivery/reproducibility/project CI.
- `docs/AI.md` — AI necessity/provider/validation policy.
- `docs/EVIDENCE.md` — только фактические результаты.
- `docs/SUBMISSION.md` — финальный operational manifest/presentation contract.
