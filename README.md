# djangotutorial (Django Polls Tutorial)

This repo follows Django’s official **Writing your first Django app** tutorial (Polls app).  
Current progress: roughly **Part 3** (views + templates + URL routing).

## What’s in here

- Django project: `mysite/`
- Django app: `mysite/polls/`
- Templates: `mysite/polls/templates/polls/`
- Main pages:
  - Polls index page lists latest questions.
  - Question detail pages (basic placeholder responses / template depending on progress).

## Requirements

- Python 3.x
- Packages in `requirements.txt`

## Setup (Windows PowerShell)

From the repo root:

```powershell
python -m venv env
.\env\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Run the app

From the `mysite/` directory (the one that contains `manage.py`):

```powershell
cd .\mysite
python manage.py runserver
```

Then open:
- `http://127.0.0.1:8000/polls/`

## Database (when you start using models/admin)

```powershell
cd .\mysite
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Admin:
- `http://127.0.0.1:8000/admin/`

## Notes

- This repo intentionally tracks tutorial code as it evolves.
- Local virtual environments (`env/`) and environment files (`.env`) are ignored via `.gitignore`.

