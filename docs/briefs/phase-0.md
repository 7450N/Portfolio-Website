# Phase 0 — Scaffolding

## Learn first (ask Gemini for resources on each, in this order)

You already know: C#/Unity, Python/FastAPI, PHP/Laravel, MySQL basics, JS/HTML/CSS, Git basics.

1. **Docker & docker-compose fundamentals** — images vs containers, ports, volumes, healthchecks; why we run MySQL in a container instead of installing it on Windows. (Biggest gap — spend the most time here.)
2. **Maven + Spring Boot project anatomy** — what `pom.xml` is (think NuGet/Unity Package Manager), what Spring Initializr generates, what `application.yml` configures. Compare to how Laravel/FastAPI projects are structured — the concepts map 1:1.
3. **Vite + React project anatomy** — what Vite is (dev server + build tool), what `npm create vite` generates, how `npm run dev` vs `npm run build` differ.
4. **Env vars & `.env` files** — why secrets/config never go in code; `.env` vs `.env.example`.
5. **Feature-branch workflow** — branch per phase, commits, what a review before merge is for.

Sanity check you're ready: you can answer — *What happens to MySQL's data when the container is deleted? What program serves localhost:5173 vs :8080? Why does the DB password come from an env var?*

## Then have Gemini build (scope contract)

Work on branch `phase-0-scaffolding`. Follow `docs/PLAN.md` (repo layout, stack). Scope is scaffolding ONLY — no DB tables, no security config, no real UI, no Dockerfile, no CI:

1. Replace `.gitignore` (Java/Maven + Node + `.env` + IDE rules).
2. Root `docker-compose.yml`: single `mysql:8.4` service — db/user `portfolio`, password from `DB_PASSWORD` (default `portfolio`), port 3306, named volume, healthcheck. Root `.env.example`.
3. `backend/` via Spring Initializr: Maven, Java 21, Spring Boot 3.x; group `com.minthawkaung`, artifact `portfolio`; deps: Web, Data JPA, Security, Validation, Actuator, MySQL Driver, Flyway. Add jjwt 0.12.x + Testcontainers (mysql, junit-jupiter) + spring-security-test (test scope). `application.yml` datasource → `jdbc:mysql://localhost:3306/portfolio`, password `${DB_PASSWORD:portfolio}`, `ddl-auto: validate`. Empty `db/migration/` folder with `.gitkeep`.
4. `frontend/` via `npm create vite@latest -- --template react-ts`; add `react-router-dom`; routes `/` (Home placeholder) and `/admin` (Admin placeholder); `src/lib/api.ts` fetch wrapper using `VITE_API_URL` (default `http://localhost:8080`); `frontend/.env.example`.
5. Placeholder dirs: `e2e/`, `load-tests/`, `.github/workflows/` (`.gitkeep` each).

## Definition of done (PowerShell)

```powershell
docker compose up -d          # mysql reaches "healthy"
cd backend; .\mvnw.cmd test   # BUILD SUCCESS (compose must be up first)
cd ..\frontend; npm run dev   # localhost:5173 shows Home; /admin shows placeholder
npm run build                 # succeeds
```

Then bring the branch back to Claude for review before merging.
