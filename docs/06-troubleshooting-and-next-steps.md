# Troubleshooting and next steps

This file focuses on “practical developer experience” issues you’re likely to hit as you continue, especially since your setup is more production-like (Postgres + `.env`) than the default tutorial (SQLite).

## 1) Running the server (the normal loop)

From repo root (PowerShell):

```powershell
.\env\Scripts\Activate.ps1
cd .\mysite
python manage.py runserver
```

Then open:

- `http://127.0.0.1:8000/polls/`
- `http://127.0.0.1:8000/admin/`

## 2) Database prerequisites (because you’re using Postgres)

Your `mysite/mysite/settings.py` uses:

- `django.db.backends.postgresql`
- env vars for connection details

Make sure:

- Postgres is installed and running
- the database exists
- credentials are correct in your `.env`

Typical `.env` keys your settings expects:

- `DB_NAME`
- `DB_USER`
- `DB_PASSWORD`
- `DB_HOST`
- `DB_PORT`

## 3) Dependency mismatch to be aware of

Your settings import `dotenv`:

- `from dotenv import load_dotenv`

And you use the Postgres backend.

For a fresh machine to reproduce your setup, you usually need these installed too:

- `python-dotenv`
- a Postgres driver (commonly `psycopg` / `psycopg[binary]`)

If you see errors like:

- `ModuleNotFoundError: No module named 'dotenv'`
- `django.core.exceptions.ImproperlyConfigured: Error loading psycopg2 or psycopg module`

…that is why.

## 4) Migrations sanity check (your “schema is ready” signal)

From `mysite/`:

```powershell
python manage.py makemigrations
python manage.py migrate
```

Expected behavior at this stage:

- `makemigrations` should say “No changes detected” (since you already have `0001_initial.py`)
- `migrate` should apply Django built-in migrations + your polls migration (if not applied yet)

## 5) Creating data for templates (admin is the easiest)

1. Create a superuser:

```powershell
python manage.py createsuperuser
```

2. Visit admin:
   - `http://127.0.0.1:8000/admin/`

3. Add at least one `Question`.
4. Add `Choice` rows (you may need to register `Choice` in admin if you want to create them there).

Why this matters:

- Your templates only show data that exists in your DB.
- If you have no questions, the index page will show “No polls are available.”

## 6) What to do right before starting Part 4

- Add `Choice` to admin (optional but convenient for data entry).
- Create a couple of questions with multiple choices.
- Then implement:
  - `results.html`
  - POST voting logic
  - redirects and error handling
  - generic views refactor

