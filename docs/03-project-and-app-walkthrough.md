# Project and app walkthrough (files in this repo)

This is a guided tour of the most important files you’ve created/modified while completing Parts 1–3.

## 1) `mysite/manage.py` — your main CLI entrypoint

`manage.py` is how you run most Django commands inside this project:

- `python manage.py runserver` — run the dev server
- `python manage.py makemigrations` — create migrations from model changes
- `python manage.py migrate` — apply migrations to the database
- `python manage.py shell` — open a Django-aware Python REPL
- `python manage.py createsuperuser` — create an admin user

If you’re used to Node scripts:

- `manage.py` is like your project’s “command runner” with framework context (similar to `next dev`, `prisma migrate`, etc.).

## 2) `mysite/mysite/settings.py` — global config (like your server config + env wiring)

Key things you’ve set up here:

### Installed apps

`INSTALLED_APPS` includes:

- `polls.apps.PollsConfig` (your app)
- Django built-in apps like `django.contrib.admin`, `auth`, `sessions`, etc.

These built-in apps power:

- admin UI
- authentication system
- sessions and messages framework
- static file plumbing

### Templates

Your config sets:

- `APP_DIRS: True`

That means Django will look for templates inside each installed app at:

- `<app>/templates/...`

In your repo:

- `mysite/polls/templates/polls/index.html`
- `mysite/polls/templates/polls/detail.html`

### Database

You configured Postgres:

- `ENGINE = 'django.db.backends.postgresql'`
- DB credentials pulled via `os.getenv(...)` after `load_dotenv()`

This is a real-world pattern:

- code stays constant
- per-environment config lives in environment variables (or `.env` during local dev)

Important implication:

- Your project will not run migrations successfully unless:
  - Postgres is running and reachable, and
  - `.env` values are set correctly.

## 3) `mysite/mysite/urls.py` — root router

This file defines project-level URL patterns.

In your repo:

- `/polls/` routes to the polls app via `include("polls.urls")`
- `/admin/` routes to Django’s admin site

This is roughly analogous to:

- `app.use('/polls', pollsRouter)`
- `app.use('/admin', adminRouter)`

Except Django’s admin is built-in.

## 4) `mysite/polls/models.py` — the data model

This file defines your database schema using Python classes.

### `Question`

- `question_text`: `CharField(max_length=200)`
- `pub_date`: `DateTimeField("date published")`
- `was_published_recently()`: helper method (used later in admin + views logic in the tutorial)

### `Choice`

- `question`: `ForeignKey(Question, on_delete=models.CASCADE)`
- `choice_text`: `CharField(max_length=200)`
- `votes`: `IntegerField(default=0)`

Key concept:

- You did not write raw SQL.
- Django uses migrations to translate these model definitions into DB tables.

## 5) `mysite/polls/migrations/` — schema history

You have `0001_initial.py`, which contains the operations to create:

- `polls_question`
- `polls_choice`

Think of migrations like:

- Prisma migrations
- Knex migrations

They make schema changes:

- explicit
- reviewable
- replayable in other environments

## 6) `mysite/polls/admin.py` — admin registration

Right now you register only:

- `Question`

So the admin can manage questions.

Later, you’ll likely:

- register `Choice`
- add inline editing for choices inside the question page
- improve list display / filters

## 7) `mysite/polls/urls.py` — app routing table

You define:

- `""` → `index`
- `"<int:question_id>/"` → `detail`
- `"<int:question_id>/results/"` → `results`
- `"<int:question_id>/vote/"` → `vote`

And you namespace them under:

- `app_name = "polls"`

That’s why templates can reverse URLs by name (no hard-coded paths).

## 8) `mysite/polls/views.py` — view functions (controller handlers)

You currently use function-based views:

### `index(request)`

- ORM query: latest 5 questions
- `render(request, "polls/index.html", context)`

### `detail(request, question_id)`

- loads a `Question` (404 if missing)
- renders `"polls/detail.html"`

### `results(...)` and `vote(...)`

- currently placeholders that return `HttpResponse`
- Part 4 converts vote into a POST handler and results into a data-driven page

## 9) Templates

### `mysite/polls/templates/polls/index.html`

- If there are questions:
  - loops through them
  - prints a link to each detail page using `{% url %}`
- Else:
  - prints “No polls are available.”

### `mysite/polls/templates/polls/detail.html`

- renders the question text
- loops through choices via `question.choice_set.all`

Right now it’s read-only.

In Part 4, this page becomes a form that:

- submits the selected choice to `/vote/` using POST
- includes a CSRF token

