# Hyne Pallets QLD — EPC v2.4

Timber inventory, production planning, and dispatch management system for Hyne Pallets Queensland.

## Stack
- **Backend**: Python / Flask / SQLite
- **Frontend**: React (CDN) / Single-page apps
- **Deploy**: Railway (auto-deploy from GitHub)

## Apps
| Path | Purpose |
|---------|---------|
| `/` | Office app — orders, planning, ops dashboard, dispatch |
| `/floor` | Floor tablet — production scanning and job tracking |
| `/driver` | Driver mobile — delivery runs and proof of delivery |
| `/chainsaw` | Chainsaw station — docking and crosscut operations |
| `/receiving` | Receiving — inbound timber and stock receipts |

## Environment Variables (Required)

| Variable | Required | Notes |
|----------|----------|-------|
| `PORT` | No | Defaults to `8080` |
| `JWT_SECRET` | **Yes** | Must be a strong random string. Server will not start without it. |
| `ADMIN_DEFAULT_PW` | Recommended | Password for the initial admin account (used on first DB creation only). |
| `DEFAULT_USER_PW` | Recommended | Password for seed test users (used on first DB creation only). |
| `SMTP_PASSWORD` | If using email | SMTP mail server password. Required for dispatch notifications. |
| `CORS_ORIGIN` | If custom domain | Comma-separated allowed CORS origins. Defaults to same-origin only. |

> **Do not commit secrets to this repo.** See [SECURITY_SETUP.md](SECURITY_SETUP.md) for setup instructions.

## Deploy to Railway
1. Connect this repo to a Railway project
2. Set **all required environment variables** listed above (see SECURITY_SETUP.md)
3. Railway auto-detects `Procfile` and deploys
4. The database is created automatically on first startup

## First Login
After deployment, log in with the admin email and the password you set via `ADMIN_DEFAULT_PW`.
Default seed users and PINs are created on first run — change them immediately via the Admin panel.

## Security Hardening (v2.4)
- All PINs hashed with PBKDF2 (auto-migrated from plaintext on startup)
- All passwords hashed with PBKDF2 (legacy SHA256 auto-upgrades on login)
- JWT tokens expire after 24 hours
- Rate limiting on all login endpoints (10 attempts / 60 minutes)
- Database error messages sanitised — raw SQL errors never sent to client
- CORS fully environment-driven — no hardcoded URLs
- Security headers: HSTS, X-Content-Type-Options, X-Frame-Options, Referrer-Policy, Permissions-Policy
- API responses include Cache-Control: no-store
- Database backup endpoint (executive-only) with auto-rotation (keeps last 7)

## Version
- v2.4.0 — Security hardening, input validation, offline sync expansion, backup system
- v2.3.0 — Kanban pipeline, docking flow, docking log, audit hardening
