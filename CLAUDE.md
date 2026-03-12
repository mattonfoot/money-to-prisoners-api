# CLAUDE.md

## Project Overview

Money to Prisoners API — Django REST Framework backend and internal admin site for the Prisoner Money suite of apps. Handles prisoner money transfers, credits, disbursements, payments, and related security/account management.

## Tech Stack

- Python 3.12+, Django, Django REST Framework
- PostgreSQL 14+
- Node.js 24 (for asset bundling only)
- Docker & Docker Compose for local services
- uWSGI in production

## Common Commands

### Setup

```bash
python3 -m venv venv && source venv/bin/activate
pip install -r requirements/dev.txt
docker-compose up -d          # start local PostgreSQL
./manage.py migrate
```

### Running the Dev Server

```bash
./run.py start --test-mode    # builds, migrates, loads test data, serves on :8000
./run.py serve                # serve only (assumes DB is ready)
```

### Running Tests

```bash
./manage.py test                                        # all tests
./manage.py test mtp_api.apps.credit.tests              # one app
./manage.py test mtp_api.apps.credit.tests.test_views   # one module
```

### Linting & Checks

```bash
flake8                            # lint (config in setup.cfg)
./manage.py check                 # Django system checks
./manage.py makemigrations --check  # verify no missing migrations
```

## Code Style

- **Linter:** flake8 — max line length 120, max complexity 15 (see `setup.cfg`)
- **Indentation:** 4 spaces for Python; 2 spaces for other files (see `.editorconfig`)
- **Trailing whitespace:** trimmed; files end with a newline

## Project Layout

```
mtp_api/
  apps/           # Django apps: account, core, credit, disbursement, mtp_auth,
                  #   notification, payment, performance, prison, security,
                  #   service, transaction, user_event_log
  settings/       # base.py, ci.py, docker.py, local.py.sample
  templates/      # Django templates
  translations/   # i18n files
  urls.py         # root URL config
requirements/     # base.txt, dev.txt, ci.txt
```

Each app follows standard Django structure: `models.py`, `views.py`, `serializers.py`, `admin.py`, `tests/`, `migrations/`.

## Testing Notes

- Tests use Django's test framework with `model-bakery` and `Faker` for data generation.
- CI runs tests in 16 parallel shards on CircleCI.
- Always run `./manage.py makemigrations --check` after model changes to verify migrations are up to date.

## Key URLs (local dev)

- Admin: http://localhost:8000/admin/
- Swagger docs: http://localhost:8000/swagger/
- ReDoc: http://localhost:8000/redoc/
