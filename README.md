# Pri-Fly workflow catalog

Каталог сценариев [Pri-Fly](https://gitlab.com/stenhigh/prifly): категории и
указатели на Git-репозитории, в которых лежат Project workflow folders.

Каталог служит только для поиска. Он не хранит bytes сценариев, ключи или
trust: установка копирует папку из указанного репозитория в tracked
`.prifly/workflows/NAME/` проекта, а доверие package по-прежнему решается
владельцем при `prifly project start`.

## Использование

```sh
prifly project workflows search --catalog https://github.com/StenHigh/prifly-workflows.git
prifly project workflows add aif-classic --catalog https://github.com/StenHigh/prifly-workflows.git
```

Команды описаны в OpenSpec change `add-project-workflow-catalog` репозитория
Pri-Fly и появятся в следующем release. До него каталог можно читать как
обычный YAML и копировать папку из `repository/path` вручную.

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
- Неизвестные поля отклоняются.

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
| `aif-classic` | software-delivery | `examples/aif-classic` в Pri-Fly `v0.4.0` |
| `aif-fanout` | software-delivery | `examples/aif-fanout` в Pri-Fly `v0.4.0` |

Оба сценария требуют skills AI Factory в host root проекта; их совместимость
с опубликованным AI Factory package отслеживается в roadmap Pri-Fly.
