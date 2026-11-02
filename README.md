<h1 align="center">Pri-Fly workflow catalog</h1>

<p align="center">
  Каталог сценариев <a href="https://github.com/StenHigh/prifly">Pri-Fly</a>:
  категории и указатели на Git-репозитории, в которых лежат Project workflow
  folders. Один <code>catalog.yaml</code>, никаких bytes сценариев, ключей или trust.
</p>

<p align="center">
  <a href="https://github.com/StenHigh/prifly-workflows/commits/main"><img src="https://img.shields.io/github/last-commit/StenHigh/prifly-workflows?label=catalog%20updated&amp;color=1f6feb" alt="catalog updated"></a>
  <img src="https://img.shields.io/badge/schema-prifly--workflow--catalog%2F1-00ADD8" alt="prifly-workflow-catalog/1">
  <img src="https://img.shields.io/badge/entries-8-4b5563" alt="8 entries">
</p>

<p align="center">
  <a href="#использование">Использование</a> ·
  <a href="#формат-catalogyaml">Формат</a> ·
  <a href="#как-добавить-сценарий">Как добавить сценарий</a> ·
  <a href="#записи">Записи</a> ·
  <a href="https://github.com/StenHigh/prifly-aif-workflows">AI Factory</a> ·
  <a href="https://github.com/StenHigh/prifly">Pri-Fly</a>
</p>

<p align="center">
  <img src="assets/readme/hero.jpg" alt="Кабина: журнал активных сценариев и их маршрутов — каталог показывает, откуда и что можно поставить" width="100%">
</p>

Каталог служит только для поиска. Установка копирует папку из указанного
репозитория в tracked `.prifly/workflows/NAME/` проекта, а доверие package
по-прежнему решается владельцем при `prifly project start`.

## Использование

Этот репозиторий встроен в Pri-Fly как каталог по умолчанию (Pri-Fly `v0.6.0`
и новее):

```sh
prifly project workflows search                 # весь каталог
prifly project workflows search review          # подстрока по имени, описанию и tags
prifly project workflows search --category software-delivery
prifly project workflows add aif-classic        # запись → .prifly/workflows/aif-classic
prifly project workflows add aif-fix            # конкретный баг; 2.0.0 требует Core >= 0.13.86 и общие схемы
prifly project workflows add aif-review-evidence # приёмка готового кода; >= 0.13.76
prifly project workflows add aif-ui-ux           # UI/UX; 2.0.0 требует Core >= 0.13.86 и общие схемы
prifly project workflows add aif-feature         # небольшая новая фича; >= 0.13.77
prifly project workflows add openclaw-test-audit # аудит тестов; без AI Factory skills
prifly project workflows update aif-classic     # к exact commit того же ref
```

Другой каталог указывается флагом `--catalog URL`. Для более старых версий
Pri-Fly `catalog.yaml` можно читать как обычный YAML и копировать папку из
`repository/path` вручную.

## Формат `catalog.yaml`

```yaml
schema_version: prifly-workflow-catalog/1
title: Pri-Fly workflow catalog
categories:
  CATEGORY:
    title: Human-facing category title
    description: One sentence about the category.
workflows:
  NAME:
    title: Human-facing workflow title
    description: One sentence about the workflow.
    category: CATEGORY
    repository: https://host/owner/repo.git   # любой Git URL без credentials
    path: relative/path/to/workflow/folder   # папка с workflow.yaml; опубликованный folder contract /1, /2 или /3
    ref: v1.2.0                              # tag, branch или commit; без ref — remote HEAD
    commit: 0123456789abcdef…                # необязательный pin: установка отказывает при другом commit
    tags: [free, form, keywords]             # необязательно
```

Правила:

- `NAME` и `CATEGORY` — строчные латинские буквы, цифры, `-` и `_`, не длиннее
  64 символов; каждое имя уникально, `category` записи обязана существовать.
- `repository` — абсолютный Git URL (`https://`, `ssh://`, `git@host:path.git`)
  без пользователя и пароля.
- Указывайте `ref` на tag и по возможности `commit`: установка закрепляет
  exact commit, а обновление сравнивает его с записанным.
- Новая версия сценария должна поднимать `package.version` в его
  `workflow.yaml`; иначе проект после обновления столкнётся с уже sealed
  package той же identity.
- Неизвестные поля отклоняются. Локальная JSON Schema для редактора —
  [`workflow-catalog-v1.schema.json`](https://github.com/StenHigh/prifly/blob/main/schemas/authoring/workflow-catalog-v1.schema.json)
  в репозитории Pri-Fly.

## Как добавить сценарий

1. Положите Project workflow folder (`workflow.yaml` с
   поддержанным `authoring` folder contract (`/1`, `/2` или `/3`), `steps/`, `schemas/`, `contexts/`,
   `workflows/`, `decisions/`) в любой публичный Git-репозиторий и отметьте
   release tag. Один репозиторий может содержать несколько папок.
2. Добавьте запись в `catalog.yaml` и, если нужно, категорию.
3. Откройте pull request с описанием, что делает сценарий и какие host skills
   ему нужны.

## Записи

| Имя | Категория | Репозиторий |
|---|---|---|
| `aif-classic` | software-delivery | `aif-classic` в [`StenHigh/prifly-aif-workflows`](https://github.com/StenHigh/prifly-aif-workflows) |
| `aif-fanout` | software-delivery | `aif-fanout` в [`StenHigh/prifly-aif-workflows`](https://github.com/StenHigh/prifly-aif-workflows) |
| `aif-profiled` | software-delivery | `aif-profiled` в [`StenHigh/prifly-aif-workflows`](https://github.com/StenHigh/prifly-aif-workflows) |
| `aif-fix` | software-delivery | `aif-fix` в [`StenHigh/prifly-aif-workflows`](https://github.com/StenHigh/prifly-aif-workflows) |
| `aif-review-evidence` | software-delivery | `aif-review-evidence` в [`StenHigh/prifly-aif-workflows`](https://github.com/StenHigh/prifly-aif-workflows) |
| `aif-ui-ux` | software-delivery | `aif-ui-ux` в [`StenHigh/prifly-aif-workflows`](https://github.com/StenHigh/prifly-aif-workflows) |
| `aif-feature` | software-delivery | `aif-feature` в [`StenHigh/prifly-aif-workflows`](https://github.com/StenHigh/prifly-aif-workflows) |
| `openclaw-test-audit` | software-delivery | `openclaw-test-audit` в [`StenHigh/prifly-aif-workflows`](https://github.com/StenHigh/prifly-aif-workflows) |

Пакеты `aif-*` требуют skills AI Factory в host root проекта; их совместимость
с опубликованным AI Factory package отслеживается в backlog репозитория
`StenHigh/prifly-aif-workflows`. Версия и commit каждой записи — в
`catalog.yaml`. Остановленный Run возобновляется тем же launch:
`prifly project continue --source-run RUN`.


Новые пакеты опубликованы в [v1.52.0](https://github.com/StenHigh/prifly-aif-workflows/releases/tag/v1.52.0):
review-evidence1.0.0, ui-ux1.0.0, feature1.2.3. При выборе начните с задачи:
готовый код — review-evidence; готовый интерфейс — ui-ux; новая небольшая фича — feature.
[Установка, примеры и границы квалификации](https://github.com/StenHigh/prifly-aif-workflows/blob/v1.52.0/docs/releases/v1.52.0.md).
В feature приёмка последовательная по умолчанию; параллельные режимы требуют
явного Codex CLI/project adapter. Параллельные UI/UX квалифицированы на1–3 кругах.
В v1.53.0 семь прежних пакетов сохранили содержимое и версии.


В [v1.53.0](https://github.com/StenHigh/prifly-aif-workflows/releases/tag/v1.53.0) добавлен **openclaw-test-audit1.0.0** — самостоятельный аудит изменённых тестов по адаптированному checklist OpenClaw. Требует Pri-Fly ≥0.13.77 и Python3, без AI Factory/OpenClaw skills. При пустой выборке AI-сессия не запускается; автоматических вставок в существующие workflow нет. [Установка и границы](https://github.com/StenHigh/prifly-aif-workflows/blob/v1.53.0/docs/releases/v1.53.0.md).


[Готовые config Python/Node/monorepo и команды preview/start/continue](https://github.com/StenHigh/prifly-aif-workflows/tree/main/examples/test-audit); [пошаговое подключение через extend.yaml](https://github.com/StenHigh/prifly-aif-workflows/tree/main/examples/test-audit-extension). В v1.54.0 пакет openclaw-test-audit сохранил версию1.0.0; готовые примеры находятся на main.


В [v1.54.0](https://github.com/StenHigh/prifly-aif-workflows/releases/tag/v1.54.0) опубликованы читаемые **aif-classic1.48.0** и **aif-profiled1.49.0**. Сложные YAML mappings раскрыты; verify-инструкция находится в Markdown. Маршруты и текст инструкций сохранены. Остальные шесть пакетов сохраняют содержимое и версии. Все восемь записей закрепляют tag v1.54.0 и commit5d4169b. [Установка, обновление и проверки](https://github.com/StenHigh/prifly-aif-workflows/blob/v1.54.0/docs/releases/v1.54.0.md); [готовый classic example](https://github.com/StenHigh/prifly-aif-workflows/tree/v1.54.0/examples/classic).


В [v1.55.0](https://github.com/StenHigh/prifly-aif-workflows/releases/tag/v1.55.0) опубликована readability-порция остальных шести пакетов: **fanout1.5.0**, **fix1.1.0**, **review-evidence1.1.0**, **ui-ux1.1.0**, **feature1.3.0**, **openclaw-test-audit1.1.0**. Сложные bindings раскрыты;34 инструкции перенесены в package-local Markdown с сохранением точных decoded bytes. Classic1.48.0 и profiled1.49.0 сохраняют bytes v1.54.0. Все восемь записей закрепляют v1.55.0/source42e60de. [Версии, установка и команды обновления](https://github.com/StenHigh/prifly-aif-workflows/blob/main/docs/releases/v1.55.0.md); [приёмка и границы](https://github.com/StenHigh/prifly-aif-workflows/blob/main/docs/changes/improve-workflow-authoring-readability/remaining-acceptance.md). Все три CI jobs прошли на official Pri-Fly0.13.82. Project extend.yaml и project/ сохраняются при update; незавершённые Runs завершите до смены author versions. Fanout не поддерживает continue; прежние no_work отказы не исправляются оформлением.


В [v1.56.0](https://github.com/StenHigh/prifly-aif-workflows/releases/tag/v1.56.0) опубликованы **classic1.49.0** и **profiled1.50.0**. Verify fail/no_work завершает Run как completed/rejected без обязательного gate; continue повторяет verify с сохранённым деревом. Эти две записи закрепляют tag v1.56.0/source1985ae1; остальные шесть сохраняют pins v1.55.0. Старые failed Runs автоматически не мигрируют. [Установка и ограничения](https://github.com/StenHigh/prifly-aif-workflows/blob/main/docs/releases/v1.56.0.md). Все три jobs CI41219942108 прошли на official Pri-Fly0.13.82; новую матрицу составляют12 cases/28 Runs.


В [v1.57.0](https://github.com/StenHigh/prifly-aif-workflows/releases/tag/v1.57.0) опубликованы
**classic/profiled/fix/UIUX2.0.0** и **feature1.4.0**. Пять записей закрепляют
source `ee76702dec0d8e0958ef068ef58074ecf2db37e9`; остальные три сохраняют pins v1.55.0.
Scoped `/5` points и public imports передают исходные report refs после repair/continue.
Для первых четырёх пакетов нужны Core ≥0.13.86 и **явная установка**
`aif:package/composition-contracts@1.2.0` из точного checkout этого выпуска.
Каталог копирует только workflow folder. Он не копирует `contracts/` и `tools/`
и не импортирует зависимости. [Полные команды](https://github.com/StenHigh/prifly-aif-workflows/blob/v1.57.0/docs/releases/v1.57.0.md)
показывают установку, native update и явную миграцию owner extensions.
Feature сохраняет прежние embedded review contracts; старые Runs не переписываются.
Новые public exports используют folder `/3`; поддержка поиска каталога со старой
версии CLI не означает поддержку каждого нового пакета. Сверяйте его требования.
