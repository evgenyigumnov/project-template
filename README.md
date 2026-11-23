# Шаблон проекта для Harness AI

Это стартовый шаблон для работы над проектом с Harness AI: готовые профили специализированных агентов в `.codex/agents/`, правила их работы и оркестрации, документы о качестве, тестировании, безопасности и развёртывании, а также заготовки для задач и UX/UI-прототипов. Скопируйте набор в репозиторий проекта и адаптируйте материалы под его цели, стек и команду.

## Начало работы с шаблоном

1. Скопируйте шаблон в репозиторий проекта ( .md файлы, .codex, prototypes и tickets папки)
2. Запустите Codex: `codex --yolo`
3. В запущеном Codex опишите свою задачу


## Содержит следующих агентов

- [`analyst`](.codex/agents/analyst.toml) — анализирует требования, бизнес-логику, предметную область и задачи.
- [`architector`](.codex/agents/architector.toml) — проектирует архитектуру системы, определяет технические решения и стандарты разработки.
- [`planner`](.codex/agents/planner.toml) — изучает репозиторий и ведёт план в `tickets/<ID>-<slug>/PLAN.md` для нового проекта и feature-задач; для обычного bugfix оркестратор подключает его отдельно, если исправление крупное, затрагивает несколько модулей или требует нескольких этапов.
- [`frontend-developer`](.codex/agents/frontend-developer.toml) — разрабатывает клиентскую часть приложения, интерфейсы и интеграцию с API.
- [`backend-developer`](.codex/agents/backend-developer.toml) — разрабатывает серверную логику, API и интеграции.
- [`database-developer`](.codex/agents/database-developer.toml) — проектирует структуру БД, пишет миграции и оптимизирует SQL-запросы.
- [`devops`](.codex/agents/devops.toml) — настраивает серверы, инфраструктуру, CI/CD и развёртывание приложений.
- [`security-engineer`](.codex/agents/security-engineer.toml) — проверяет безопасность кода, архитектуры и инфраструктуры, выявляет уязвимости.
- [`ux-ui-expert`](.codex/agents/ux-ui-expert.toml) — проектирует пользовательские интерфейсы, прототипы и сценарии взаимодействия.
- [`unit-tester`](.codex/agents/unit-tester.toml) — создаёт, поддерживает и запускает модульные тесты.
- [`ui-tester`](.codex/agents/ui-tester.toml) — создаёт и запускает UI/E2E-тесты, проверяет пользовательские сценарии.
- [`technical-writer`](.codex/agents/technical-writer.toml) — пишет и поддерживает документацию, справку, руководства и инструкции.
- [`code-reviewer`](.codex/agents/code-reviewer.toml) — проверяет качество кода, соблюдение стандартов и корректность изменений.

## Содержит следующие документы

Документы, уже включённые в шаблон:

- [`AGENTS.md`](AGENTS.md) — правила оркестрации, назначения ролей и обязательных проверок.
- [`CONTRIBUTING.md`](CONTRIBUTING.md) — порядок разработки, проверок и ревью.
- [`CONVENTIONS.md`](CONVENTIONS.md) — общие соглашения по коду и архитектурным границам.
- [`DEPLOYMENT.md`](DEPLOYMENT.md) — целевые профили, окружения и требования к развёртыванию.
- [`SECURITY.md`](SECURITY.md) — требования безопасной разработки и работы с уязвимостями.
- [`TESTING.md`](TESTING.md) — уровни тестирования, правила запуска и отчётности.
- [`API.md`](API.md) — контракт API, если приложение его предоставляет.
- [`tickets/README.md`](tickets/README.md) — формат папок задач, обязательные материалы и условия для отчётов; поддерживает `analyst`.
- [`prototypes/README.md`](prototypes/README.md) — структура и правила подготовки UX/UI-прототипов.

Документы, которые агенты создают или обновляют по мере необходимости:

- **`DOMAIN.md`, `BUSINESS_RULES.md`, `REQUIREMENTS.md`, `GLOSSARY.md`** — предметная область, бизнес-правила, требования и термины; ведёт `analyst`.
- **`ARCHITECTURE.md`, `docs/adr/*.md`, `docs/DESIGN.md`** — архитектура, решения и описание дизайна системы; ведёт `architector`.
- **`DATABASE.md`, `docs/DB_PERFORMANCE.md`** — схема данных, миграции и производительность БД; ведёт `database-developer`.
- **`docs/UI_GUIDELINES.md`, `docs/USER_FLOWS.md`** — общие правила интерфейсов и повторно используемые пользовательские потоки; ведёт `ux-ui-expert`.
- **`docs/UNIT_TESTING.md`, `docs/UI_TESTING.md`** — подробные соглашения по модульным и UI/E2E-тестам; ведут `unit-tester` и `ui-tester`.
- **`README.md`, `docs/GETTING_STARTED.md`, `docs/USER_GUIDE.md`, `docs/HELP.md`, `docs/TROUBLESHOOTING.md`** — вводные инструкции, справка, руководства и помощь в устранении проблем; ведёт `technical-writer`.
- **`docs/CODE_REVIEW.md`** — критерии ревью изменений; ведёт `code-reviewer`.
- **`prototypes/<ID>-<slug>/README.md`** — описание экранов, действий, состояний и допущений для UX/UI-прототипа, если его удобно оформить в Markdown; готовит `ux-ui-expert`.
- **`tickets/<ID>-<slug>/TASK.md`** — исходный запрос и контекст каждой задачи; создаёт оркестратор.
- **`tickets/<ID>-<slug>/PLAN.md`** — этапы, зависимости и фактический прогресс по плану задачи; готовит и обновляет `planner` на основе подтверждённых результатов исполнителей.
- **`tickets/<ID>-<slug>/ANALYSIS.md`** — анализ требований и предметной области; готовит `analyst`.
- **`tickets/<ID>-<slug>/UNIT_TEST_REPORT.md`** — сценарии, тесты, команды и результаты unit-проверок; готовит `unit-tester`.
- **`tickets/<ID>-<slug>/UI_TEST_REPORT.md`** — сценарии, тесты, команды и результаты UI/E2E-проверок; готовит `ui-tester`.
- **`tickets/<ID>-<slug>/TECHNICAL_WRITING_REPORT.md`** — проверенные источники, обновлённые документы или обоснование, почему правки не нужны; готовит `technical-writer`.
- **`tickets/<ID>-<slug>/CODE_REVIEW.md`** — письменное ревью изменений, если оно поручено; готовит `code-reviewer`.
- **`tickets/<ID>-<slug>/SECURITY_REVIEW.md`** — результаты проверки безопасности; готовит `security-engineer`.
