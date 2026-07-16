# Портфолио технического писателя / Technical Writer Portfolio

**Анатолий Дудов / Anatoliy Dudov**

📱 Telegram: [@Object_NULL1332](https://t.me/Object_NULL1332)
💼 LinkedIn: [anatoliy-dudov](https://www.linkedin.com/in/anatoliy-dudov/)
🐙 GitHub: [anatoliy-dudov](https://github.com/anatoliy-dudov)

---

## 🇷🇺 О проекте

Это портфолио технического писателя, собранное как сайт на **MkDocs (Material)**
с поддержкой двух языков — русского (по умолчанию) и американского английского.

**Сайт:** https://anatoliy-dudov.github.io/documentation/

### Что внутри

- **API-документация** — два подхода к документированию API:
  - автогенерируемый референс на основе спецификации OpenAPI (TaskFlow API);
  - вручную написанная, аннотированная документация платёжного API
    в духе популярных платёжных провайдеров (PayFlow API) — с быстрым стартом,
    аутентификацией, кодами ошибок и вебхуками.
- **База знаний** — примеры статей: инструкция, troubleshooting-статья, FAQ,
  онбординг-гайд.
- **Проблем-менеджмент** — плейбук реагирования на инциденты, база известных
  ошибок (KEDB), шаблон постмортема (RCA) и матрица эскалации.
- **Ресурсы** — глоссарий терминов, чек-листы для ревью документации и
  подборка дальнейшего чтения (стандарты, стилевые гайды, инструменты).

Все примеры — авторские демо-материалы для показательных целей: они написаны
с нуля для конкретных учебных продуктов (TaskFlow, PayFlow) и не являются
копией документации реальных сервисов.

### Как запустить локально

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
mkdocs serve
```

Сайт будет доступен на http://127.0.0.1:8000/.

### Деплой

Публикация на GitHub Pages выполняется автоматически через
`.github/workflows/deploy.yml` при пуше в ветку `main`
(используется `mkdocs gh-deploy`).

---

## 🇬🇧 About this project

This is a technical writer's portfolio built as an **MkDocs (Material)** site
with support for two languages — Russian (default) and American English.

**Live site:** https://anatoliy-dudov.github.io/documentation/

### What's inside

- **API documentation** — two documentation approaches:
  - an auto-generated reference built from an OpenAPI specification
    (TaskFlow API);
  - hand-written, annotated documentation for a payment API in the style
    of popular payment providers (PayFlow API) — quickstart, authentication,
    error codes, and webhooks.
- **Knowledge base** — sample articles: a how-to guide, a troubleshooting
  article, an FAQ, and an onboarding guide.
- **Problem management** — an incident response playbook, a Known Error
  Database (KEDB), a postmortem (RCA) template, and an escalation matrix.
- **Resources** — a glossary, documentation-review checklists, and a
  further-reading list (standards, style guides, tools).

All samples are original demo content written for fictional teaching products
(TaskFlow, PayFlow) and are not copies of any real service's documentation.

### Run locally

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
mkdocs serve
```

The site will be available at http://127.0.0.1:8000/.

### Deployment

Publishing to GitHub Pages is automated via `.github/workflows/deploy.yml`
on every push to `main` (using `mkdocs gh-deploy`).

## Лицензия / License

MIT — см. [LICENSE](LICENSE).
