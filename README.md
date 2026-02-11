---
type: home
title: Content repo
description: Контент-репозиторий для Notepub
---

# np-content

Шаблон контент-репозитория для сайта на Notepub + GitHub Pages.

## Как это работает

1. Вы редактируете markdown-файлы в этом репозитории (удобно через Obsidian).
2. Делаете commit + push в `main`.
3. Workflow `trigger-np-deploy.yml` отправляет `repository_dispatch` в ваш сайт-репозиторий.
4. В сайт-репозитории запускается сборка/индексация/деплой на GitHub Pages.

## Что хранить в репозитории

- Только контент (`*.md`) в корне репозитория.
- Frontmatter должен соответствовать `rules.yaml` вашего сайт-репозитория.

Минимальный пример файла:

```md
---
type: article
slug: my-note
title: Моя заметка
description: Короткое описание
---

Текст заметки.
```

## Первый запуск

1. Создайте новый репозиторий на GitHub из этого шаблона (`Use this template`).
2. В созданном репозитории добавьте Repository variable:
`NP_SITE_REPO` = `owner/repo` вашего сайт-репозитория (например, `myname/my-site`).
3. Добавьте Repository secret:
`NP_DEPLOY_TOKEN` — GitHub token с правом вызывать `repository_dispatch` в сайт-репозитории.
4. Сделайте первый push контента в `main`.

## Obsidian

- Открывайте как vault корень этого репозитория.
- Для one-click сценария используйте Obsidian Git (`Commit-and-sync`).
