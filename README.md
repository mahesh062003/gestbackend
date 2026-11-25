# Gestura — authentication backend

Flask JWT authentication service for [Gestura](https://github.com/mahesh062003/gestura-fe),
a sign-language translation app. Deployed with gunicorn.

## API

| Route | Method | Purpose |
|---|---|---|
| `/` | GET | Health check |
| `/api/signup` | POST | Create an account — `{name, email, password}` |
| `/api/login` | POST | Exchange credentials for a JWT — `{email, password}` |
| `/api/verify` | POST | Validate a token and return its user — `{token}` |

Passwords are hashed with bcrypt and never stored or returned in plaintext.
Tokens are HS256 and expire one hour after issue. `/api/verify` separates an
expired token from a malformed one, and both from a valid token whose user no
longer exists.

## Known limitation

**Accounts are held in an in-memory dict and do not survive a restart.** This
is a demonstration auth layer, not a production one — `users = {}` needs
replacing with a real database before it is relied upon.

## Running it

```bash
pip install -r requirements.txt
export SECRET_KEY="a-real-random-secret"   # falls back to an insecure default
python app.py                              # http://localhost:5000
```

`SECRET_KEY` must be set in any deployed environment. The fallback exists so
the app starts locally; tokens signed with it could be forged by anyone.

## Deployment

`Procfile` runs `gunicorn app:app` on Python 3.9.7 (`runtime.txt`).

## Stack

Python · Flask · Flask-Bcrypt · PyJWT · Flask-CORS · gunicorn

---

## Use

© 2026 Mahesh Veeraiah R. All rights reserved.

Published for review and assessment. Not licensed for reuse, redistribution or
modification — please get in touch about any other use.
