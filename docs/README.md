# Django Polls Tutorial Notes (Parts 1–3)

These notes document what you have built so far in this repo while following Django’s official “Writing your first Django app” tutorial (the **Polls** app).

They are written for your background as a production MERN/Next.js developer (TypeScript, REST APIs, auth, CI/CD), so they include “translation layers” between the Django way and what you already know.

## Reading order

1. `docs/01-where-you-are-now.md` — quick snapshot of the current project state (what exists, what works, what’s placeholder).
2. `docs/02-django-mental-model-for-mern.md` — Django concepts mapped to MERN/Next.js mental models.
3. `docs/03-project-and-app-walkthrough.md` — a guided tour of the actual files in this repo.
4. `docs/04-part3-templates-urls-views.md` — deep dive into Part 3 (templates + URL routing + view functions).
5. `docs/05-part4-preview-forms-and-generic-views.md` — what you’ll do next (Part 4) and how it will change your code.
6. `docs/06-troubleshooting-and-next-steps.md` — common issues + a practical checklist before continuing.

## What “done with Part 3” means (in this repo)

Part 3’s core learning goals are already present:

- The Polls app is wired into the project URL router (`mysite/mysite/urls.py` includes `polls.urls`).
- The app has URL patterns (`mysite/polls/urls.py`) that pass parameters like `question_id`.
- Views query the database via the ORM (`Question.objects.order_by(...)`) and render templates.
- Templates live under `mysite/polls/templates/polls/` and use Django Template Language (DTL) for loops, conditionals, and `{% url %}` reversing.

Some Part 4 features are still placeholders (especially voting / POST handling), which is expected since you haven’t started Part 4 yet.

