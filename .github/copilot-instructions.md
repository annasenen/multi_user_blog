<!-- .github/copilot-instructions.md - Project-specific guidance for AI coding agents -->
# Project: multi_user_blog — Copilot Instructions

Purpose
- Provide concise, actionable rules and examples so an AI coding agent can be productive immediately in this repository.

Big picture
- Minimal Flask app using a package-level global `app` defined in `travelcompanyblog/__init__.py`.
- App entrypoint: `app.py` imports `from travelcompanyblog import app` and runs `app.run(debug=True)`.
- Logical feature partitions:
  - `travelcompanyblog/core/` — simple `core` blueprint (see `core/views.py`).
  - `travelcompanyblog/blog_posts/` — placeholder package for post-related views/forms.
  - `travelcompanyblog/users/` — placeholder package for user auth/forms.
- Templates are in the top-level `templates/` directory; static assets in `static/`.

What to look for (important files)
- `app.py` — run the development server; no app factory pattern used.
- `travelcompanyblog/__init__.py` — defines `app = Flask(__name__)` (where global app lives).
- `travelcompanyblog/core/views.py` — example blueprint declaration:
  ```py
  core = Blueprint('core', __name__)

  @core.route('/')
  def index():
      return render_template('index.html')
  ```
- `travelcompanyblog/models.py` — currently empty (no DB configured here).
- `requirements.txt` — source of runtime dependencies; use `pip install -r requirements.txt`.

Conventions & patterns in this repo
- Blueprints: each subpackage (`core`, `blog_posts`, `users`) contains a `views.py` that defines a `Blueprint` instance named after the package (e.g., `core`). Follow this naming and export pattern for new features.
- Views return `render_template(...)` and use `forms.py` modules next to `views.py` for WTForms (files exist but are currently empty).
- Single global Flask app: code expects `from travelcompanyblog import app` instead of an app factory. Changes should respect this unless a refactor is explicitly requested.

Development & common commands (PowerShell)
- Create environment and install deps:
  ```powershell
  python -m venv .venv
  .\.venv\Scripts\Activate.ps1
  pip install -r requirements.txt
  ```
- Run dev server (project uses `app.run(debug=True)`):
  ```powershell
  python app.py
  ```
- Alternative (set FLASK_APP and use flask run):
  ```powershell
  $env:FLASK_APP = 'app.py'; flask run
  ```

Project-specific guidelines for edits
- When adding a new feature package (e.g., `travelcompanyblog/comments`):
  - Create `views.py` with `Blueprint('comments', __name__)` and `forms.py` (if needed).
  - Import and register the blueprint in `travelcompanyblog/__init__.py` (current code does not auto-register blueprints).
    Example registration (if asked to modify):
    ```py
    from .core.views import core
    app.register_blueprint(core)
    ```
- Follow existing minimal style: small functions per route, templates in `templates/`, forms in `forms.py`.
- Avoid introducing an app-factory or DB layer without an explicit refactor task — the repo currently expects a single `app` object.

Integration points & external dependencies
- WTForms (form classes expected under each package's `forms.py`) — check `requirements.txt` for exact package names.
- No SQLAlchemy or DB config found in `models.py`; persistence may be out-of-scope or handled elsewhere.

Examples (copyable patterns found here)
- Blueprint definition (from `travelcompanyblog/core/views.py`):
  ```py
  from flask import Blueprint, render_template

  core = Blueprint('core', __name__)

  @core.route('/')
  def index():
      return render_template('index.html')
  ```

Where to look for more context
- `templates/` — template names referenced by views.
- `requirements.txt` — dependency list and versions.
- Add tests only if the feature requires behavior verification; no test harness currently exists.

If you need to change architecture
- Ask before converting to an app-factory pattern or adding DB integrations. Provide a short migration plan.

Questions for the maintainer (please answer to refine these instructions)
- Do you want automatic blueprint registration in `travelcompanyblog/__init__.py` or manual imports in `app.py`?
- Is there a preferred DB or ORM to use when adding persistence (e.g., SQLAlchemy)?

End.
