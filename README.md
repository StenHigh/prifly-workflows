<h1 align="center">Pri-Fly workflow catalog</h1>

<p align="center">
  Каталог сценариев <a href="https://gitlab.com/stenhigh/prifly">Pri-Fly</a>:
  категории и указатели на Git-репозитории, в которых лежат Project workflow
  folders. Один <code>catalog.yaml</code>, никаких bytes сценариев, ключей или trust.
</p>

<p align="center">
  <a href="https://github.com/StenHigh/prifly-workflows/commits/main"><img src="https://img.shields.io/github/last-commit/StenHigh/prifly-workflows?label=catalog%20updated&amp;color=1f6feb" alt="catalog updated"></a>
  <img src="https://img.shields.io/badge/schema-prifly--workflow--catalog%2F1-00ADD8" alt="prifly-workflow-catalog/1">
  <img src="https://img.shields.io/badge/entries-2-4b5563" alt="2 entries">
</p>

<p align="center">
  <a href="#использование">Использование</a> ·
  <a href="#формат-catalogyaml">Формат</a> ·
  <a href="#как-добавить-сценарий">Как добавить сценарий</a> ·
  <a href="#записи">Записи</a> ·
  <a href="https://github.com/StenHigh/prifly-aif-workflows">AI Factory</a> ·
  <a href="https://gitlab.com/stenhigh/prifly">Pri-Fly</a>
</p>

<p align="center">
  <img src="assets/readme/hero.jpg" alt="Кабина: журнал активных сценариев и их маршрутов — каталог показывает, откуда и что можно поставить" width="100%">
</p>

Каталог служит только для поиска. Установка копирует папку из указанного
репозитория в tracked `.prifly/workflows/NAME/` проекта, а доверие package
по-прежнему решается владельцем при `prifly project start`.

## Использование

Этот репозиторий встроен в Pri-Fly как каталог по умолчанию (в версиях после
`v0.5.0`, где появились команды `project workflows`):

```sh
prifly project workflows search                 # весь каталог
prifly project workflows search review          # подстрока по имени, описанию и tags
prifly project workflows search --category software-delivery
prifly project workflows add aif-classic        # запись → .prifly/workflows/aif-classic
prifly project workflows update aif-classic     # к exact commit того же ref
```

Другой каталог указывается флагом `--catalog URL`. До релиза с этими
командами `catalog.yaml` можно читать как обычный YAML и копировать папку из
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
    path: relative/path/to/workflow/folder   # папка с workflow.yaml и marker prifly-project-workflow/1
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
  [`workflow-catalog-v1.schema.json`](https://gitlab.com/stenhigh/prifly/-/blob/main/schemas/authoring/workflow-catalog-v1.schema.json)
  в репозитории Pri-Fly.

## Как добавить сценарий

1. Положите Project workflow folder (`workflow.yaml` с
   `authoring: prifly-project-workflow/1`, `steps/`, `schemas/`, `contexts/`,
   `workflows/`, `decisions/`) в любой публичный Git-репозиторий и отметьте
   release tag. Один репозиторий может содержать несколько папок.
2. Добавьте запись в `catalog.yaml` и, если нужно, категорию.
3. Откройте pull request с описанием, что делает сценарий и какие host skills
   ему нужны.

## Записи

| Имя | Категория | Репозиторий |
|---|---|---|
| `aif-classic` | software-delivery | `aif-classic` в [`StenHigh/prifly-aif-workflows`](https://github.com/StenHigh/prifly-aif-workflows) `v1.0.0` |
| `aif-fanout` | software-delivery | `aif-fanout` в [`StenHigh/prifly-aif-workflows`](https://github.com/StenHigh/prifly-aif-workflows) `v1.0.0` |

Оба сценария требуют skills AI Factory в host root проекта; их совместимость
с опубликованным AI Factory package отслеживается в backlog репозитория
`StenHigh/prifly-aif-workflows`.
