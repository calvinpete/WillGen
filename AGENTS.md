# AGENTS.md

## Cursor Cloud specific instructions

### Overview

WillGen is a Django 3.2.5 REST API project (will generation app). It is currently a scaffold with only the default Django admin at `/admin/`.

### Services

| Service | How to run |
|---|---|
| Django dev server | `source /workspace/venv/bin/activate && cd /workspace/willApp && python manage.py runserver 0.0.0.0:8000` |
| PostgreSQL | `sudo pg_ctlcluster 16 main start` (must be running before Django) |

### Non-obvious setup notes

- **Python 3.12 compatibility**: The pinned `psycopg2-binary==2.8.6` and `Markdown==3.3.4` in `requirements.txt` are incompatible with Python 3.12. Install upgraded versions (`psycopg2-binary>=2.9.9` and `Markdown>=3.4`) after running `pip install -r requirements.txt`.
- **PostgreSQL 16 schema permissions**: PostgreSQL 15+ revoked default CREATE on the `public` schema for non-superusers. After creating a database user, you must run: `GRANT ALL ON SCHEMA public TO <user>;` on the target database.
- **Environment variables**: The Django app uses `python-decouple` and reads from a `.env` file in `/workspace/willApp/.env`. Required vars: `SECRET_KEY`, `DATABASE_URL`, `DEBUG` (see `sample_env.txt` for format).
- **Static files directory**: `mkdir -p /workspace/willApp/static/production` must exist before `collectstatic` will work (referenced by `STATICFILES_DIRS` in settings).
- **Procfile bug**: The `Procfile` references `will.wsgi` but the correct module is `willApp.wsgi`.

### Commands reference

- **Lint**: `flake8 --max-line-length=120 --exclude=venv,migrations .` (from `willApp/`)
- **Tests**: `python manage.py test` (from `willApp/`)
- **Django checks**: `python manage.py check`
- **Migrations**: `python manage.py migrate`
- **Superuser**: `DJANGO_SUPERUSER_USERNAME=admin DJANGO_SUPERUSER_EMAIL=admin@willgen.dev DJANGO_SUPERUSER_PASSWORD=admin123 python manage.py createsuperuser --noinput`
