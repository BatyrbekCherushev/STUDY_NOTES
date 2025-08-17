# AUTO DEPLOY OF MKDOCS DOCUMENTATION TO GITHUB PAGES


На прикладі мого проекту

## 1️⃣ Структура проекту
```mkdocs
/beetroot_project            ← - корньовий каталог проекту
│   .gitignore
│   mkdocs.yml               ←  файл налаштування проекту мкдокс
├───docs                     ←  каталог проекту мкдокс
│   ├───index.md             ←  стартова сторінка
│   ├───DOCUMENTATION/
│   │       page_profile.md
│   │       page_vocabulary.md
│   │       page_armory.md
│   │       page_arena.md
│   └───COMMANDS/
│           commands_django.md
│           commands_mkdocs.md
│
├───.github/workflows
│       docs-deploy.yml       ← GitHub Actions для автодеплою
│
│
├───project                     ← папка проекту Джанго
│   └─── ...                 ← код Django (models, views, templates, static)

```
> Project tree

**Важливі моменти:**

* index.md завжди у корені docs/ → стартова сторінка.

* Інші сторінки у підпапках → додаються у nav в mkdocs.yml.

* Папка .github/workflows/ містить YAML файл з налаштуванням CI/CD для автодеплою.

## 2️⃣ mkdocs.yml 
(приклад для моєї структури)

```yaml
site_name: Heroes of Word
site_url: https://batyrbekcherushev.github.io/beetroot_project/
repo_url: https://github.com/BatyrbekCherushev/beetroot_project

nav:
  - ІНТРО: index.md
  - Документація проекта:
      - Сторінка "профіль": DOCUMENTATION/page_profile.md
      - Сторінка "словник": DOCUMENTATION/page_vocabulary.md
      - Сторінка "зброярня": DOCUMENTATION/page_armory.md
      - Сторінка "арена": DOCUMENTATION/page_arena.md
  - Команди:
      - DJANGO: COMMANDS/commands_django.md
      - MKDOCS: COMMANDS/commands_mkdocs.md

theme:
  name: material
  language: uk
  features:
    - search.highlight
    - navigation.footer
    - navigation.instant
    - navigation.tracking
    - navigation.tabs
    - navigation.tabs.sticky
    - navigation.indexes
    - navigation.top
  palette:
    - scheme: default
      toggle:
        icon: material/brightness-7
        name: Темний
    - scheme: slate
      toggle:
        icon: material/brightness-4
        name: Світлий
```

**Пояснення:**

* site_url → обов’язково для GitHub Pages, щоб стилі та скрипти правильно підвантажувались.
* repo_url → додає кнопку "View on GitHub" у матеріальній темі.
* nav → структура меню на сайті. Перша сторінка (index.md) стає стартовою.
* theme → налаштування теми Material для MkDocs.


## 3️⃣ GitHub Actions
(.github/workflows/docs-deploy.yml)

> Цей каталог, підкаталог і файл створюєте самостійно в корньовому каталозі вашого проекту

* назва самого файла може бути довільна, головне правильне розширення і вміст 

```yaml

name: Deploy Docs

on:
  push:
    branches:
      - main  # або master, якщо твоя основна гілка master

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write  # ✅ дозвіл на пуш в gh-pages
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      - name: Install dependencies
        run: pip install mkdocs mkdocs-material

      - name: Deploy to GitHub Pages
        run: mkdocs gh-deploy --force
```

🧾**Пояснення:**

* Деплой відбувається автоматично при пуші в main.
* permissions: contents: write → обов’язково для пушу в gh-pages.
* mkdocs gh-deploy --force → збудує сайт і запише його у гілку gh-pages.
* Після першого деплою GitHub автоматично створює гілку gh-pages.

- on: push — тригер, який запускає деплой при кожному пуші в main
- actions/checkout — завантажує твій репозиторій
- setup-python — встановлює Python
- pip install mkdocs-material — ставить тему
- mkdocs gh-deploy — деплоїть сайт на gh-pages

## ️4️⃣ Важливі моменти для GitHub Pages

1. Перевірка Pages:
    * У Settings → Pages → джерело: gh-pages branch / root.
2. Файл .nojekyll:
    * MkDocs створює його автоматично під час gh-deploy.
    * Потрібен, щоб GitHub Pages не намагався обробляти сайт через Jekyll.
3. Стартова сторінка:
    * Завжди index.md у docs/
4. Шляхи до ресурсів:
    * Завдяки site_url в `mkdocs.yml` стилі та скрипти підтягуються правильно на GitHub Pages.

## 5️⃣ Після деплою

* Стартова сторінка:
>https://batyrbekcherushev.github.io/beetroot_project/ → index.md
* Інші сторінки доступні через меню навігації.
* Всі стилі та скрипти Material підвантажуються коректно.
* При наступному пуші в main → деплой відбувається автоматично.

## 6️⃣ Налаштування на GitHub

1. Перевір основну гілку
    * Зайди у твій репозиторій → Code → гілка повинна бути main (або master).
    * Саме ця гілка буде тригером для GitHub Actions.

2. Перевірка Pages
    * Зайди в Settings → Pages.
    * Source (джерело): обери gh-pages branch / root.
    * Збережи зміни.
3. Перевірка секретів
    * Для простого деплою через gh-deploy секрети не потрібні, бо permissions: contents: write дозволяє GitHub Actions пушити без токена.
    * Якщо використовуєш кастомний токен → додаєш у Settings → Secrets → Actions.
4. Перевірка mkdocs.yml
    * site_url має бути вказано саме на GitHub Pages URL:
    * repo_url → на твій GitHub репозиторій:
    ```yaml
    site_url: https://batyrbekcherushev.github.io/beetroot_project/
    repo_url: https://github.com/BatyrbekCherushev/beetroot_project
    ```
5. Пуш файлів у репозиторій
    * Додай нові файли у git:
    ```bash
    git add .
    git commit -m "Setup MkDocs auto-deploy"
    git push origin main
    ```
    * Це запустить GitHub Actions → деплой в gh-pages.
        * сайт білдиться автоматично
        * гілка gh-pages створюється автоматично
6. Перевірка результату
    * Після успішного деплою Actions:
    * Стартова сторінка: https://batyrbekcherushev.github.io/beetroot_project/
    * Інші сторінки доступні через меню навігації.
    * Якщо стилі не підвантажуються → перевір site_url в mkdocs.yml.


🔐 1. Увімкни Allow GitHub Actions to write to the repository

Перейди в налаштування (Settings) репозиторію:

- Відкрий репозиторій на GitHub
- Перейди в Settings → Actions → General
- Прокрути до секції Workflow permissions
- Вибери: [x] Read and write permissions