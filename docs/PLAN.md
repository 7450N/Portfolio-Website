# Portfolio Website — Architecture & Roadmap

> This file is the source of truth for architecture contracts (folder layout, API design,
> data model, env-var names). Any implementation — human or AI — must follow it.
> Deviations get adopted here deliberately or reverted; never silently drifted.

## Context

Personal portfolio website for Min Thaw Kaung showcasing summary, skills, work experience,
and projects. All content is managed through a hidden admin panel (add/edit/delete without
touching code), protected by a login only the owner has. The React frontend follows the
owner's reference design screenshots.

**Working model:** Gemini implements each phase and teaches the owner while doing so;
Claude supervises — writes the phase briefs, reviews every diff, runs builds/tests,
guards these contracts, and runs learning checkpoints before a phase merges.

## Stack decisions

- Frontend: React (Vite + TypeScript) on **Vercel** (free, CDN, auto-deploy, HTTPS)
- Backend: **Java 21 + Spring Boot 3** (Maven), Docker image on **Render free tier**
- Database: **MySQL 8** — local dev via Docker; production on **Aiven free tier** (TLS)
- Images: **pasted URLs** in the admin form (no upload pipeline)
- CI/CD: **GitHub Actions**; testing: **JUnit 5 + Testcontainers**, **Playwright** (E2E), **k6** (load)
- AWS deliberately removed for v1 (ops complexity without product value). Everything is
  Dockerized, so a later AWS migration (EC2 + RDS) is a hosting swap, not a rewrite.

## Software Architecture

```
                    ┌─────────────────────────────────────────┐
                    │                 VISITOR                 │
                    └────────┬───────────────────┬────────────┘
                             │ HTTPS             │ HTTPS (API calls)
                             ▼                   ▼
            ┌────────────────────────┐  ┌──────────────────────────────┐
            │   VERCEL (free)        │  │   RENDER (free web service)  │
            │  React SPA (static)    │  │  Spring Boot API (Docker)    │
            │  - Public portfolio    │  │  - REST /api/**              │
            │  - /admin (SPA route,  │  │  - Spring Security + JWT     │
            │    JWT-gated)          │  │  - 512MB RAM → tuned JVM     │
            └────────────────────────┘  └──────────────┬───────────────┘
                                                       │ TLS (JDBC)
                                                       ▼
                                        ┌──────────────────────────────┐
                                        │   AIVEN (free tier)          │
                                        │   Managed MySQL 8            │
                                        └──────────────────────────────┘

  Project images: hosted externally (GitHub raw / YouTube thumbnails / etc.)
  — admin form stores pasted URLs, visitors' browsers load them directly.

  DEV & DELIVERY:
  GitHub repo ──push/PR──> GitHub Actions CI
                           (JUnit + Testcontainers MySQL, lint, tsc, build,
                            Playwright E2E against docker-compose stack)
                      └──> on main, CI green ──> curl Render Deploy Hook
                                                 └─> Render builds Dockerfile
                                                     & rolls out new version
  Vercel auto-builds & deploys frontend on every push to main
  UptimeRobot (free) pings /actuator/health every 10 min → no cold starts
  k6 (local / manual workflow_dispatch) ──gentle load tests──> Render API
```

## Repository layout (monorepo)

```
Portfolio-Website/
├─ frontend/                 # React + Vite + TypeScript
├─ backend/                  # Spring Boot 3 (Maven, Java 21)
├─ e2e/                      # Playwright tests (TypeScript)
├─ load-tests/               # k6 scripts (smoke.js, load.js, spike.js)
├─ .github/workflows/        # ci.yml (tests + deploy-hook step), k6.yml
├─ docs/                     # this plan + phase briefs
├─ docker-compose.yml        # local dev stack (MySQL + backend)
└─ README.md                 # architecture diagram + run/deploy instructions
```

## Data model (MySQL, managed by Flyway migrations)

- `profile` (single row): name, headline, summary, email, phone, linkedin_url, github_url
- `skill_category`: name, sort_order, items (JSON array of strings)
- `work_experience`: title, company, start_date, end_date (nullable = present), bullets (JSON), sort_order
- `project`: title, tech_tags (JSON), bullets (JSON), links (JSON array of {label,url}), image_urls (JSON array of pasted URLs), featured (bool), sort_order
- `admin_user`: username, bcrypt password hash (seeded from env vars on first boot)

JSON columns (JPA `@Convert` attribute converters) keep the schema small — no child tables
for bullets/links/tags.

## API design

| Method | Path | Auth | Purpose |
|---|---|---|---|
| GET | `/api/portfolio` | public | Everything in one call (profile, skills, experience, projects) |
| POST | `/api/auth/login` | public | username+password → JWT (HS256, ~24h expiry) |
| POST/PUT/DELETE | `/api/admin/profile`, `/api/admin/skills/**`, `/api/admin/experience/**`, `/api/admin/projects/**` | JWT | CRUD |
| GET | `/actuator/health` | public | health check (CI smoke, Docker healthcheck, UptimeRobot, k6) |

## Security

- Spring Security, stateless: JWT filter; `/api/admin/**` requires `ROLE_ADMIN`; everything else public read-only.
- Admin credentials (`ADMIN_USERNAME`, `ADMIN_PASSWORD_HASH`) and JWT secret (`JWT_SECRET`) via env vars — never committed. Render dashboard for prod, gitignored `.env` locally (`.env.example` committed).
- CORS: allow only the Vercel domain + localhost.
- Rate-limit the login endpoint (simple in-memory bucket) — the only brute-forceable surface.

## Phases

| # | Branch | Scope |
|---|---|---|
| 0 | `phase-0-scaffolding` | Monorepo folders, .gitignore, docker-compose (MySQL), Spring Boot + Vite scaffolds |
| 1 | `phase-1-backend` | Flyway schema+seed, entities/converters, /api/portfolio, JWT security, admin CRUD, JUnit + Testcontainers tests |
| 2 | `phase-2-frontend` | Public portfolio page per reference design; /admin login + CRUD dashboard |
| 3 | `phase-3-docker` | Multi-stage backend Dockerfile (512MB-tuned JVM), full compose stack |
| 4 | `phase-4-ci` | GitHub Actions: backend tests, frontend lint/build, Playwright E2E, deploy-hook step |
| 5 | (manual) | Aiven + Render + Vercel + UptimeRobot setup; GitHub Secrets |
| 6 | `phase-6-k6` | k6 smoke/load/spike scripts + manual workflow; document findings |
| 7 | `phase-7-polish` | README, badges, real content entered in prod |

### Per-phase workflow (learn first → build → review)

1. Claude writes `docs/briefs/phase-N.md`: a **learn-first topic list** + a short scope contract.
2. Owner asks Gemini for resources on those topics and learns them BEFORE any code is written.
3. Gemini implements on the phase branch, following this plan's contracts and the brief's scope.
4. Claude reviews the diff, runs builds/tests, checks contracts, flags fixes (owner relays them to Gemini).
5. Learning checkpoint (below), then merge to main when review + CI are green.

### Learning checkpoints

| Phase | After it, you should be able to explain… |
|---|---|
| 0 | What docker-compose does; why MySQL runs in a container locally; what Vite is |
| 1 | Controller → Service → Repository → DB flow; Flyway migrations; JWT login end-to-end; why bcrypt |
| 2 | How React fetches/renders API data; how the SPA guards /admin; where the JWT lives and why |
| 3 | What a multi-stage Dockerfile does; why the JVM needs memory flags in a 512MB container |
| 4 | What each CI job verifies; why E2E runs against a real composed stack; what Testcontainers spins up |
| 5 | How Vercel/Render/Aiven find each other via env vars; why CORS config exists |
| 6 | What p95 latency means; the measured limits of the free tier |

## Verification

- **Local**: `docker compose up` → `mvn verify` green → Vite app renders seeded data → admin login → create/edit/delete a project and watch the public page update.
- **CI**: all jobs green on a PR; review the Playwright report artifact.
- **Prod**: Vercel URL loads; API calls hit `https://….onrender.com`; full admin CRUD round-trip in prod; `k6 run load-tests/smoke.js` passes; no cold start on first visit (UptimeRobot warm).
