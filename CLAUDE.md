# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

**Spendly** — a personal expense tracker web app built with Flask + Jinja2 + SQLite. Amounts are in Indian rupees (₹).

This is a **step-by-step teaching scaffold**, not a finished app. The front-facing pages (landing, register, login) are fully built and styled, but the core logic is intentionally left as numbered exercises. `app.py` and `database/db.py` contain placeholder routes/stubs labeled with the step that implements them (e.g. `"Add expense — coming in Step 7"`). When implementing, replace these placeholders — don't treat them as finished behavior.

## Commands

The environment is Windows / PowerShell with a project-local virtualenv in `venv/`.

```powershell
# Activate the venv (do this first)
.\venv\Scripts\Activate.ps1

# Install / update dependencies
pip install -r requirements.txt

# Run the app — serves on http://127.0.0.1:5001 with debug + auto-reload
python app.py

# Run the full test suite (pytest + pytest-flask are installed)
pytest

# Run a single test file / test
pytest tests/test_auth.py
pytest tests/test_auth.py::test_login_success
```

Note: no `tests/` directory exists yet — tests are added as features are built. The app runs on **port 5001** (not the Flask default 5000).

## Architecture

- **`app.py`** — the entire Flask application: app instance + all route definitions. Routes are split into two groups: working routes (`/`, `/register`, `/login`, all currently GET-only, rendering templates) and placeholder routes returning literal strings. Implementing a step means fleshing out its placeholder here.

- **`database/`** — a package (`__init__.py` is empty). `db.py` is a **stub** to be implemented in Step 1: it must expose `get_db()` (SQLite connection with `row_factory` and foreign keys enabled), `init_db()` (`CREATE TABLE IF NOT EXISTS`), and `seed_db()` (sample dev data). The SQLite file is `expense_tracker.db` (gitignored).

- **`templates/`** — Jinja2. `base.html` is the layout (navbar, footer, `content`/`head`/`scripts` blocks, loads Google Fonts + CSS + JS); all pages `{% extends "base.html" %}`. Use `url_for('<route_fn>')` for links and `url_for('static', filename=...)` for assets.

- **`static/`** — `css/style.css` (complete design system) and `js/main.js` (empty stub for feature JS).

### Key wiring detail

The `register.html` and `login.html` forms **POST** to `/register` and `/login` and expect an `error` template variable (`{% if error %}`). But `app.py` currently defines those routes as GET-only with no POST handling. Implementing auth means adding `methods=["GET", "POST"]`, processing form fields (`name`, `email`, `password`), and passing `error=...` back into `render_template` on failure. The template contract already exists — match it.

## Conventions

- **Styling via CSS custom properties.** `static/css/style.css` defines all colors, fonts, radii, and widths as `:root` variables (`--ink`, `--paper`, `--accent`, `--danger`, `--font-display`, `--radius-md`, etc.). Reuse these tokens rather than hardcoding values. Fonts: DM Serif Display (headings) + DM Sans (body).
- **Currency is ₹ (rupees)** throughout copy and mock data.
- Placeholder route strings follow the pattern `"<Feature> — coming in Step N"` — a map of what's left to build.
