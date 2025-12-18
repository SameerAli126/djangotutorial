# Where you are now (Project snapshot)

This file is a “status page” of your repo as it exists today.

## Your background (so the notes match how you think)

From your CV: you’re a MERN/Next.js developer with strong TypeScript, REST API, auth (JWT), and production delivery experience (deployments, CI/CD, performance optimization).

That matters because Django is a **batteries-included** framework: it gives you routing, ORM, migrations, admin UI, templating, security defaults, and project structure conventions out of the box. You’re learning how those pieces fit together, which is similar to learning a full opinionated “stack” (like Next.js App Router + Prisma + NextAuth), but with Django’s style and Python.

## Repository structure

At the repo root:

- `env/` — your local virtual environment (kept out of Git via `.gitignore`)
- `mysite/` — Django project workspace (contains `manage.py`)
- `requirements.txt` — pinned dependencies for this tutorial
- `.gitignore` — ignores venvs, secrets, and personal docs

Inside `mysite/`:

- `mysite/manage.py` — CLI entry point for Django management commands
- `mysite/mysite/` — the Django *project package* (global settings + root URL config)
- `mysite/polls/` — the Django *app* you’re building (models, views, URLs, templates)

## What works right now (behavior)

Assuming your database is configured and migrated:

- `GET /polls/`
  - Queries the latest 5 `Question` rows from the database (ordered by `pub_date` descending).
  - Renders the template at `mysite/polls/templates/polls/index.html`.
  - Shows links to each question’s detail page using `{% url 'polls:detail' question.id %}`.

- `GET /polls/<question_id>/`
  - Loads that `Question` by primary key using `get_object_or_404`.
  - Renders the template at `mysite/polls/templates/polls/detail.html`.
  - Displays the question text and iterates over its related `Choice` objects via `question.choice_set.all`.

- `GET /polls/<question_id>/results/` and `GET /polls/<question_id>/vote/`
  - Currently return placeholder `HttpResponse` strings.
  - These are the endpoints that will get “real” behavior in Part 4 (forms + POST + redirects).

## What you’ve built in the database layer

In `mysite/polls/models.py` you have:

- `Question` model
  - `question_text` (string)
  - `pub_date` (datetime, labeled “date published” in admin forms)
  - `was_published_recently()` helper method (currently a simple “within last day” check)

- `Choice` model
  - `question` ForeignKey → `Question` with `on_delete=models.CASCADE`
  - `choice_text` (string)
  - `votes` (integer, default 0)

Your migration `mysite/polls/migrations/0001_initial.py` corresponds to these models.

## Admin integration status

In `mysite/polls/admin.py` you register:

- `Question`

So you can manage questions in the Django admin UI.

You have not (yet) registered `Choice`, and you also haven’t enabled “inline choices” editing under questions. That’s normal for this stage; the tutorial iterates on admin configuration later.

## Notable differences vs the official tutorial

Your project has a few meaningful differences from the upstream tutorial that are worth being aware of:

1. **Database backend**: you configured **PostgreSQL** in `mysite/mysite/settings.py` instead of using SQLite.
   - This is more “production-like” and matches your background.
   - It also means you need Postgres running and you need a valid `.env` file with DB credentials.

2. **Environment loading**: you call `load_dotenv()` and read DB settings from environment variables.
   - This matches typical production patterns (12-factor config).

3. **Dependencies**: `requirements.txt` currently lists Django + core dependencies only.
   - If you rely on `python-dotenv` and a Postgres driver (e.g., `psycopg`), they should also be listed for reproducible setup. See `docs/06-troubleshooting-and-next-steps.md`.

## Your “Part 4” starting line

You’re perfectly positioned to start Part 4:

- You have real models and migrations.
- You can render pages with data-driven templates.
- Your URLs pass parameters to views.

Part 4 will teach you how to accept user input (voting), handle POST requests safely, update the database, and then refactor function-based views into class-based generic views.

