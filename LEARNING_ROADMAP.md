# Learning Roadmap — Portfolio Website

This is the study plan for building **this** project: a portfolio website with a public side (summary, education, work experience, projects) and a private admin panel where **only you** can log in and add, update, or delete content — no code edits needed.

It is written for someone starting from zero with this stack. Each phase tells you:

- **What to learn** — the specific topics, not everything that exists
- **Why this project needs it** — so nothing feels abstract
- **Where to learn it** — free, official-first resources
- **Time estimate** — assuming part-time study (~1–2 hours/day)
- **"You're ready when…"** — a self-check before moving on

Work through the phases **in order**. They mirror the order the app will be built in.

---

## 1. What you're building

```
                    ┌─────────────────────────────────────────┐
                    │              Your visitors               │
                    └───────────────────┬─────────────────────┘
                                        │ HTTPS
                                        ▼
   ┌──────────────────────────────────────────────────────────┐
   │  React frontend (Vite)                                   │
   │  • Public pages: summary, education, work, projects      │
   │  • /admin: login page + edit forms (only for you)        │
   └───────────────────┬──────────────────────────────────────┘
                       │ JSON over HTTP (REST API)
                       ▼
   ┌──────────────────────────────────────────────────────────┐
   │  Spring Boot backend (Java)                              │
   │  • GET endpoints  → public, anyone can read              │
   │  • POST/PUT/DELETE → locked behind login (JWT)           │
   │  • Spring Security checks every request                  │
   └───────────────────┬──────────────────────────────────────┘
                       │ SQL
                       ▼
   ┌──────────────────────────────────────────────────────────┐
   │  MySQL database                                          │
   │  • tables: profile, education, work_experience, project  │
   └──────────────────────────────────────────────────────────┘

   Wrapped around all of it:
   • Docker        → backend + MySQL run the same on your laptop,
                     free hosting, or AWS later
   • GitHub Actions → every push runs JUnit 5, Playwright, k6 tests
                     and deploys automatically (CI/CD)
```

The key idea behind "edit without touching code": your experience data lives in the **database**, not in the React code. The React app *asks* the backend for the data every time the page loads. When you log into the admin panel and click "Add experience", the frontend sends the new entry to the backend, which saves it to MySQL. Refresh the public page — it's there.

---

## 2. Learning phases

### Phase 0 — Foundations (Git, the web, terminal)

**~1 week**

You'll use these every single day of this project, so they come first.

**Learn:**
- Git: `clone`, `add`, `commit`, `push`, `pull`, branches, and what a pull request is
- How the web works: what happens when a browser requests a page; what a server is
- HTTP basics: methods (`GET`, `POST`, `PUT`, `DELETE`), status codes (200, 401, 404, 500), headers
- What JSON looks like and why APIs use it
- Terminal basics: moving around folders, running commands, reading error output

**Why this project needs it:** every feature you build ships through Git and GitHub. The whole frontend↔backend conversation is HTTP requests carrying JSON. `GET /api/experience` = "give me the list", `DELETE /api/experience/3` = "remove entry 3".

**Resources:**
- [GitHub's Getting Started guides](https://docs.github.com/en/get-started) — hands-on Git + GitHub
- [Pro Git book, chapters 1–3](https://git-scm.com/book/en/v2) — free, official
- [MDN: How the web works](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)
- [MDN: An overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview)

**You're ready when you can:**
- [ ] Create a branch, commit a change, push it, and open a pull request on GitHub
- [ ] Explain the difference between `GET` and `POST`, and what a 401 vs 404 means
- [ ] Read a JSON snippet and say what data it contains

---

### Phase 1 — Frontend: HTML/CSS/JS → React

**~3–4 weeks** (the biggest phase if you've never written JavaScript)

**Learn:**
1. HTML + CSS essentials: page structure, flexbox, responsive basics (~1 week)
2. JavaScript essentials: variables, functions, arrays/objects, `map`/`filter`, promises and `async/await`, `fetch` (~1–1.5 weeks)
3. React with Vite: components, props, `useState`, `useEffect`, rendering lists, controlled form inputs, calling an API with `fetch` (~1–1.5 weeks)
4. React Router: multiple pages (`/`, `/admin`) in one app

**Why this project needs it:** the public portfolio is React components rendering lists of data (`experiences.map(...)`). The admin panel is React **forms** — form handling and state are not optional extras here, they *are* the admin panel. `useEffect` + `fetch` is how pages load your data from the backend.

**Resources:**
- [MDN Learn web development](https://developer.mozilla.org/en-US/docs/Learn_web_development) — HTML, CSS, JS in one structured path
- [react.dev — Learn React](https://react.dev/learn) — the official tutorial; excellent, do it start to finish
- [Vite guide](https://vite.dev/guide/) — just the "Getting Started" page; Vite is the tool that runs/builds the React app
- [React Router docs](https://reactrouter.com/) — only the basic routing tutorial

**Skip for now:** Redux, Next.js, TypeScript, CSS frameworks debates. Plain React + plain CSS is enough.

> **Note on design:** you said you'll find a reference design and share it later — good plan. While learning, don't burn time making practice apps pretty. Structure first; styling gets applied when you have the reference.

**You're ready when you can:**
- [ ] Build a small React app that fetches a list from a public API (e.g. a fake todo API) and displays it
- [ ] Build a form that adds an item to a list on screen
- [ ] Have two pages with React Router and navigate between them

---

### Phase 2 — Backend: Spring Boot

**~3 weeks**

**Learn:**
1. Java refresher if rusty: classes, interfaces, collections, generics, annotations (what `@Something` means) (~0.5–1 week)
2. Maven: what `pom.xml` is, how dependencies and `mvn test` / `mvn package` work (1 day, learn as you go)
3. Spring Boot core: what it is, `@RestController`, `@GetMapping`/`@PostMapping`/etc., `@PathVariable`, `@RequestBody`, application.properties (~1 week)
4. Spring Data JPA: `@Entity`, repositories (`JpaRepository`), how Java objects map to database rows (~1 week)
5. Validation (`@Valid`, `@NotBlank`) and the idea of DTOs (the objects your API sends/receives vs. your database entities)

**Why this project needs it:** the backend is a set of REST endpoints — `GET /api/education` (public), `POST /api/education` (admin only) — each backed by a JPA entity and repository. This phase is the heart of the whole project.

**Resources:**
- [dev.java — official Java learning path](https://dev.java/learn/) — for the refresher
- [Spring guide: Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/) — do this one first, it's ~30 min
- [Spring guide: Accessing Data with MySQL](https://spring.io/guides/gs/accessing-data-mysql/) — almost exactly this project's backend in miniature
- [Spring Boot reference docs](https://docs.spring.io/spring-boot/) — as a lookup, not a read-through
- [start.spring.io](https://start.spring.io/) — how real Spring projects are generated; play with it

**You're ready when you can:**
- [ ] Create a Spring Boot app from start.spring.io and run it
- [ ] Write a controller with GET/POST/PUT/DELETE endpoints for one entity (e.g. "Book") storing data via JPA
- [ ] Test your endpoints with a tool like `curl` or Postman and explain each request/response

---

### Phase 3 — Database: SQL + MySQL

**~1–1.5 weeks** (overlaps naturally with Phase 2)

**Learn:**
- SQL basics: `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `WHERE`, `ORDER BY`
- Tables, columns, data types, primary keys; what a foreign key is
- Running MySQL locally (easiest: via Docker once you reach Phase 5 — until then, a local install is fine)
- How this project's schema looks: a `profile` table (your summary), and `education`, `work_experience`, `project` tables — each row = one entry you'll manage from the admin panel

**Why this project needs it:** "add/update/delete without touching code" literally means rows in these tables. JPA writes most SQL for you, but when something looks wrong you must be able to open the database and check what's actually stored.

**Resources:**
- [SQLBolt](https://sqlbolt.com/) — free interactive SQL lessons, ideal starting point
- [MySQL Tutorial (official docs)](https://dev.mysql.com/doc/refman/8.4/en/tutorial.html)

**You're ready when you can:**
- [ ] Create a table, insert rows, query them with a `WHERE` filter, update and delete a row
- [ ] Sketch the 4 tables of this project with sensible columns (e.g. `work_experience`: id, company, role, start_date, end_date, description)

---

### Phase 4 — Security: the "admin, only me" part

**~1.5–2 weeks** (conceptually the hardest phase — go slow, it matters most)

**Learn:**
- Authentication vs authorization ("who are you" vs "what may you do")
- Password hashing: why passwords are stored as BCrypt hashes, never plain text
- JWT (JSON Web Tokens): login returns a signed token; the frontend sends it with every admin request; the backend verifies it
- Spring Security basics: the filter chain idea, permitting public `GET`s while requiring auth on `POST`/`PUT`/`DELETE`
- CORS: why the browser blocks your React app (one domain) from calling your API (another domain) until the backend explicitly allows it
- Secrets hygiene: DB passwords and JWT signing keys live in **environment variables**, never in code or Git

**Why this project needs it:** this is your #1 stated requirement — "admin access, only I can edit, I need it secure." Concretely: one admin account (username + BCrypt-hashed password, seeded from environment variables), a `/api/auth/login` endpoint that returns a JWT, and Spring Security rejecting any write request without a valid token. CORS will be the first confusing error you hit when frontend and backend talk — learning it now saves hours later.

**Resources:**
- [JWT introduction (jwt.io)](https://jwt.io/introduction) — short and clear
- [Spring Security reference](https://docs.spring.io/spring-security/reference/) — read the "Servlet Applications → Getting Started" and authentication sections
- [Spring guide: Securing a Web Application](https://spring.io/guides/gs/securing-web/)
- [MDN: CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- Baeldung's Spring Security + JWT tutorials (search "baeldung spring security jwt") — the de-facto standard walkthroughs

**Skip for now:** OAuth2 / "Login with Google", refresh-token rotation, roles/permissions systems. One admin user with JWT is the right size for this project.

**You're ready when you can:**
- [ ] Explain, out loud, the full journey: login form → password checked against hash → JWT returned → JWT sent in `Authorization` header → Spring Security allows the `POST`
- [ ] Explain why the JWT secret and DB password must be environment variables
- [ ] Say what a CORS error in the browser console means and where it gets fixed (hint: backend)

---

### Phase 5 — Docker

**~1 week**

**Learn:**
- Images vs containers; what a `Dockerfile` is
- Building and running: `docker build`, `docker run`, port mapping (`-p 8080:8080`), env vars (`-e`)
- Multi-stage builds (build the Java app in one stage, run it in a slim one)
- Docker Compose: one `docker-compose.yml` starting backend + MySQL together, with a volume so DB data survives restarts

**Why this project needs it:** "pack with Docker so I can upgrade to AWS" — a Docker image is exactly the portable unit both your free host (Render) and AWS run. Compose also gives you a one-command local dev environment (`docker compose up` = app + database running).

**Resources:**
- [Docker: Get Started guide](https://docs.docker.com/get-started/) — the official workshop, do parts 1–9
- [Docker Compose overview](https://docs.docker.com/compose/)

**Skip for now:** Kubernetes, Docker Swarm, container networking deep-dives.

**You're ready when you can:**
- [ ] Write a Dockerfile for a Spring Boot jar and run it in a container, reaching it from your browser
- [ ] Write a compose file that starts your app + MySQL and have the app connect to the DB by service name

---

### Phase 6 — Testing: JUnit 5, Playwright, k6

**~1.5–2 weeks**

Three tools, three different jobs:

| Tool | What it tests | Example in this project |
|---|---|---|
| **JUnit 5** | Java code, from inside | "POST /api/education without a token returns 401" |
| **Playwright** | The real app, through a browser | "Admin logs in, adds an experience, it appears on the public page" |
| **k6** | Performance under load | "100 visitors hitting the homepage — does the API stay fast?" |

**Learn:**
- JUnit 5: `@Test`, assertions, running tests with Maven; then Spring Boot's `@SpringBootTest` / `MockMvc` for testing endpoints
- Playwright: writing a browser test (navigate, click, fill form, expect text), running headed vs headless
- k6: writing a small JS load script, virtual users, thresholds (e.g. "95% of requests under 500ms")

**Why this project needs it:** you asked for CI/CD — tests are what CI actually *runs*. Without them the pipeline is just "compile and hope." The security tests (writes rejected without a token) double as proof your #1 requirement holds.

**Resources:**
- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/) — sections 1–4 are enough to start
- [Spring guide: Testing the Web Layer](https://spring.io/guides/gs/testing-web/)
- [Playwright docs: Getting started](https://playwright.dev/docs/intro)
- [Grafana k6 docs](https://grafana.com/docs/k6/latest/) — "Get started" section

**You're ready when you can:**
- [ ] Write a JUnit test for a Spring endpoint that checks both the happy path and a 401
- [ ] Write a Playwright test that fills a login form and asserts what appears next
- [ ] Run a k6 script with 10 virtual users against a local endpoint and read the summary output

---

### Phase 7 — CI/CD: GitHub Actions

**~1 week**

**Learn:**
- What CI/CD means: every push → build + tests run automatically; merges to main → deploy automatically
- GitHub Actions vocabulary: workflow, job, step, trigger (`on: push`), the YAML file in `.github/workflows/`
- Reading a failed run's logs to find *which* test broke
- GitHub Secrets: how deploy keys/API tokens are stored (never in the YAML itself)

**Why this project needs it:** the pipeline for this repo will be roughly: push → run JUnit tests → build Docker image → run Playwright against the built app → deploy frontend to Vercel and backend to Render → (optionally) run k6 as a scheduled check.

**Resources:**
- [GitHub Actions documentation](https://docs.github.com/en/actions) — "Quickstart" and "Understanding GitHub Actions"

**You're ready when you can:**
- [ ] Add a workflow to any toy repo that runs tests on every push, and make it fail once on purpose so you learn to read the logs
- [ ] Explain what a GitHub Secret is and why the deploy token goes there

---

### Phase 8 — Hosting & deployment

**~0.5–1 week** (mostly clicking through dashboards once everything above exists)

#### Recommended free setup (you asked which is easy *and* useful)

| Piece | Host | Why |
|---|---|---|
| React frontend | **[Vercel](https://vercel.com/docs)** | Genuinely free, connects to GitHub in minutes, auto-deploys on push, fast CDN. The industry default for React hobby projects. |
| Spring Boot (Docker) | **[Render](https://render.com/docs)** free tier | Runs your actual Docker image — the same one you'd later ship to AWS, so nothing is throwaway. Simple dashboard. |
| MySQL | **[Aiven](https://aiven.io/docs)** free tier | A real managed MySQL for free; you just paste its connection URL into Render as an env var. |

**Honest caveats:** Render's free tier puts your backend to sleep after ~15 minutes of no traffic — the first visitor after that waits up to a minute while it wakes. That's normal for free tiers and fine for a portfolio. Aiven's free MySQL is small (enough for this project many times over).

**Alternative:** [Railway](https://railway.com/) hosts frontend + backend + MySQL in one dashboard, which is a simpler mental model — but it's a one-time trial credit, not a permanent free tier. Good to know it exists; not the recommendation.

**The AWS path later:** because the backend is a Docker image and *all* configuration (DB URL, passwords, JWT secret) comes from environment variables, moving to AWS is: push the image to ECR → run it on App Runner or ECS → point it at an RDS MySQL → set the same env vars. No code changes. That's the payoff of Phases 4 and 5.

**You're ready when you can:**
- [ ] Explain where each of the three pieces lives and how the frontend knows the backend's URL (env var at build time)
- [ ] Explain why switching hosts requires changing env vars, not code

---

## 3. What to SKIP (for now)

Rabbit holes that will eat weeks and add nothing to this project:

- **Kubernetes** — Docker Compose is all you need until you have real traffic
- **Microservices / Spring Cloud** — this is one backend; keep it one backend
- **Redux / Zustand / state libraries** — React's built-in state is plenty here
- **TypeScript** — worth learning eventually, but it's an extra learning curve on top of JS; add it later
- **Next.js / server-side rendering** — plain React + Vite is simpler and sufficient
- **OAuth2 / social login** — you are the only user; one JWT-protected admin account is correct
- **NoSQL / Redis / caching layers** — MySQL alone handles a portfolio's traffic effortlessly
- **Terraform / infrastructure-as-code** — relevant at the AWS stage, not before

---

## 4. Timeline at a glance

Part-time (~1–2 hrs/day), phases in order:

| Phase | Topic | Estimate |
|---|---|---|
| 0 | Git, HTTP, terminal | ~1 week |
| 1 | HTML/CSS/JS → React | ~3–4 weeks |
| 2 | Spring Boot | ~3 weeks |
| 3 | SQL + MySQL | ~1–1.5 weeks (overlaps 2) |
| 4 | Security (JWT, Spring Security) | ~1.5–2 weeks |
| 5 | Docker | ~1 week |
| 6 | JUnit 5, Playwright, k6 | ~1.5–2 weeks |
| 7 | GitHub Actions | ~1 week |
| 8 | Hosting | ~0.5–1 week |
| | **Total** | **~3–4 months part-time** |

Two rules that make the estimates real:

1. **Type, don't watch.** For every hour of reading/videos, spend an hour writing code yourself. Every "You're ready when" checkbox is something you *do*, not something you've seen done.
2. **Build tiny throwaways per phase.** A 20-line React fetch app, a "Book" CRUD API, a Dockerfile for hello-world. Small, disposable, yours. The portfolio itself comes after — and you don't need to finish all 9 phases before starting it. Phases 0–4 are enough to begin building for real; 5–8 can be learned as they're bolted on.

---

## 5. What happens next

When you've worked through the early phases (0–4 especially), come back and we build the real thing together, in the same order you learned it:

1. Spring Boot API + MySQL schema + admin JWT security
2. React public pages + admin panel — **styling waits for the reference design you'll pick**
3. Dockerfile + Docker Compose
4. JUnit 5 / Playwright / k6 test suites
5. GitHub Actions pipeline
6. Deploy: Vercel + Render + Aiven

Because you'll understand each layer, you'll be able to review, question, and change what gets generated — which was the whole point of learning first.
