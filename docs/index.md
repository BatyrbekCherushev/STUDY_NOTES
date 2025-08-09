## mkdocs commands:

>python -m mkdocs serve

>python -m mkdocs build

>python -m mkdocs gh-deploy

У корені репозиторію створи файл:
.github/workflows/deploy.yml


```yaml
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches:
      - main  # або master, залежно від твоєї гілки

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repo
      uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'

    - name: Install dependencies
      run: |
        pip install mkdocs-material

    - name: Deploy to GitHub Pages
      run: |
        mkdocs gh-deploy --force
```
🧾 Пояснення
- on: push — тригер, який запускає деплой при кожному пуші в main
- actions/checkout — завантажує твій репозиторій
- setup-python — встановлює Python
- pip install mkdocs-material — ставить тему
- mkdocs gh-deploy — деплоїть сайт на gh-pages


🔐 1. Увімкни Allow GitHub Actions to write to the repository
Перейди в налаштування репозиторію:
- Відкрий репозиторій на GitHub
- Перейди в Settings → Actions → General
- Прокрути до секції Workflow permissions
- Вибери: [x] Read and write permissions




