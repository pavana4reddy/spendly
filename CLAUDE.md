# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project context

"Spendly" — a Flask + SQLite personal expense tracker. This is a **student learning project built incrementally in numbered steps**. Much of the code is intentionally unimplemented:

- `app.py` placeholder routes have comments like `"Add expense — coming in Step 7"`, `"Edit expense — coming in Step 8"`. These are not bugs; they mark which step the student is expected to implement.
- `database/db.py` is a stub with comments describing the expected API (`get_db()`, `init_db()`, `seed_db()`) — Step 1 work.
- Auth routes (`/register`, `/login`, `/logout`, `/profile`) currently render templates or return strings; password hashing, sessions, and form POST handling are also future steps.

**Do not implement future steps unprompted.** When the user asks for help, scope changes tightly to the step they name. Adding "the rest" of the app would skip the pedagogy.

## Run / develop

```powershell
# from the project root (the inner expense-tracker/ directory)
.\venv\Scripts\Activate.ps1     # venv is checked into the repo
pip install -r requirements.txt # if deps change
python app.py                   # serves on http://localhost:5001 (NOT 5000)
```

The dev server runs on **port 5001**, not Flask's default 5000 — set explicitly in `app.py`.

## Tests

`pytest` and `pytest-flask` are in `requirements.txt` but **no `tests/` directory exists yet**. When tests are added, run them with `pytest` from the project root; run a single test with `pytest path/to/test_file.py::test_name`.

## Architecture notes

- **Templates inherit from `templates/base.html`**, which provides the navbar, footer, and loads `static/css/style.css` + `static/js/main.js` globally. Page-specific CSS goes in `{% block head %}` (see `landing.html` loading `landing.css`).
- **`base.html` uses `url_for('landing')`, `url_for('login')`, `url_for('register')` directly** — renaming any of those view functions in `app.py` breaks every page. The footer links to `/terms` and `/privacy` as hardcoded paths, not `url_for`.
- **SQLite, single-file DB**: `expense_tracker.db` is gitignored and created on first `init_db()` run. The expected pattern (per the `db.py` stub) is connection-per-request with `row_factory` and foreign keys enabled.
- **No application factory, no blueprints** — everything is registered on a module-level `app` in `app.py`. Keep new routes there unless restructuring is the explicit task.

## Conventions worth preserving

- Currency is rendered as **₹ (INR)** throughout templates and mock UI — match this when adding new views.
- The brand name "Spendly" and the `◈` glyph appear in the navbar, footer, and titles. Keep them in sync if renaming.
