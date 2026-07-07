# FirstShift — fresher job portal

A job portal for freshers (entry-level candidates), now with a Python (Flask)
backend and a SQLite database.

## What's in here

```
fresher-job-portal/
├── backend/
│   ├── app.py            Flask app: serves the frontend + the /api/* endpoints
│   ├── database.py       SQLite schema + seed data (12 sample jobs)
│   ├── requirements.txt
│   └── firstshift.db     created automatically on first run
├── index.html             Home page (fetches featured jobs from the API)
├── jobs.html               Browse/filter jobs (fetches + filters via the API)
├── job-details.html        Single job page (fetched by ?id=, has an Apply button)
├── login.html / register.html
└── assets/
    ├── style.css
    └── script.js           All API calls live here (FirstShiftAPI object)
```

## Running it

```bash
cd backend
pip install -r requirements.txt
python app.py
```

Then open **http://localhost:5000** — Flask serves both the API and the
static frontend, so there's nothing extra to configure.

The SQLite database (`backend/firstshift.db`) is created and seeded with 12
fresher-friendly jobs automatically the first time you run the app. To wipe
and reseed it at any point:

```bash
cd backend
python database.py
```

## API reference

| Method | Route                        | Description                                   |
|--------|-------------------------------|------------------------------------------------|
| GET    | `/api/jobs`                   | List jobs. Query params: `q`, `location`, `category` (repeatable), `type` (repeatable), `exp` (repeatable), `sort` (`recent` \| `stipend-high` \| `stipend-low`) |
| GET    | `/api/jobs/<id>`               | Full details for one job                       |
| POST   | `/api/auth/register`           | `{name, email, password}` → creates account + logs in |
| POST   | `/api/auth/login`              | `{email, password}` → logs in                   |
| POST   | `/api/auth/logout`             | Logs out                                        |
| GET    | `/api/auth/me`                 | Current logged-in user, or `null`               |
| POST   | `/api/jobs/<id>/apply`         | Apply to a job (requires login)                 |
| GET    | `/api/my/applications`         | Jobs the current user has applied to            |

Sessions are cookie-based (Flask's built-in signed session cookie) — no
external session store required.

## Notes

- Set `new-gen-st` as an environment variable before deploying
  anywhere real; the default in `app.py` is only for local development.
- This uses Flask's built-in dev server. For production, run it behind
  a proper WSGI server (e.g. `gunicorn app:app`) and a reverse proxy.
