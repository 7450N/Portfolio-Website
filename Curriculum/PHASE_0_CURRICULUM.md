# Phase 0 Curriculum — Foundations (Terminal, Git & GitHub, HTTP & JSON)

A 7-day plan, ~1–2 hours per day. If a day takes longer, stretch it — finishing the exercises matters more than finishing on schedule. Everything here is free.

**How to use this:**
1. Each day has **Watch/Read**, **Do**, and **Self-check** sections. The **Do** section is the real learning — never skip it.
2. Keep a simple text file called `notes.md` and write 3–5 bullet points of what you learned each day. (By Day 4 this file becomes part of an exercise.)
3. When stuck, ask Claude. At the end of each day there's a suggested question to bring back to me — I'll teach, quiz you, or review what you did.

**Setup for Windows users:** install [Git for Windows](https://git-scm.com/downloads) — it includes **Git Bash**, a terminal where all the commands in this plan work exactly as written. Use Git Bash for everything in this curriculum. (Mac/Linux users: your built-in Terminal is fine.)

---

## Day 1 — The terminal, part 1

**Goal:** stop fearing the black window. Navigate folders and manage files without a mouse.

**Watch/Read:**
- [Command Line Crash Course — Traversy Media (YouTube, ~35 min)](https://www.youtube.com/watch?v=yz7nYlnXLfE)
- Skim as reference: [freeCodeCamp — Command Line for Beginners (handbook)](https://www.freecodecamp.org/news/command-line-for-beginners/)

**Do:**
1. Open your terminal (Git Bash on Windows).
2. Using **only** commands — no file explorer, no mouse:
   - `pwd` to see where you are, `ls` to list files
   - Make a folder for all your learning work: `mkdir learning` then `cd learning`
   - Inside it create `mkdir day1` → `cd day1` → create a file: `touch hello.txt`
   - Write into it: `echo "my first terminal day" > hello.txt` and read it back with `cat hello.txt`
   - Copy it (`cp hello.txt copy.txt`), rename it (`mv copy.txt renamed.txt`), delete it (`rm renamed.txt`)
   - Go up one level (`cd ..`) and look around (`ls`)
3. Do the loop again from memory the next morning — 5 minutes, no notes.

**Self-check — you can:**
- [ ] Explain what `pwd`, `ls`, `cd`, `mkdir`, `touch`, `cp`, `mv`, `rm`, `cat` each do
- [ ] Get from any folder to any other folder using only `cd` and `ls`

**Ask Claude:** "Quiz me on basic terminal commands" — I'll fire 10 quick scenarios at you.

---

## Day 2 — The terminal, part 2

**Goal:** the skills you'll actually use daily as a developer: paths, reading output, editing config-like files.

**Watch/Read:**
- Optional deep-dive (pick 30–45 min of it, don't binge): [The 50 Most Popular Linux & Terminal Commands — freeCodeCamp/Colt Steele (YouTube, 5 hr — reference, not homework)](https://www.youtube.com/watch?v=ZtqBQ68cfJc)

**Do:**
1. Learn the difference between an **absolute path** (`/home/you/learning/day1`) and a **relative path** (`../day1`). Practice: from inside `day1`, `cat` a file in another folder using both kinds of path.
2. Create `notes.md` inside `learning/` and add your Day 1–2 bullet points using a terminal editor: `nano notes.md` (save: Ctrl+O, Enter; exit: Ctrl+X).
3. Practice reading errors: run `cd folder-that-does-not-exist` and `cat nope.txt` on purpose. Read the messages slowly. This habit — *actually reading the error* — will save you hundreds of hours across this whole project.
4. Try `history` to see everything you've typed, and the ↑ arrow to reuse commands. Try `Tab` to autocomplete file names — from now on, always use Tab.

**Self-check — you can:**
- [ ] Explain absolute vs relative paths and what `..` and `.` mean
- [ ] Open, edit, and save a file with nano
- [ ] Read an error message and say which part tells you what went wrong

**Ask Claude:** "Give me 5 broken terminal commands and let me diagnose the errors."

---

## Day 3 — Git locally

**Goal:** version control on your own machine: repo, commit, history, undo.

**Watch/Read:**
- [Git & GitHub Crash Course 2025 — Traversy Media (YouTube)](https://www.youtube.com/watch?v=vA5TTz6BXhY) — watch up to the point where it moves to GitHub; the rest is Day 4.
- Reference: [freeCodeCamp — Git & GitHub beginner-friendly handbook](https://www.freecodecamp.org/news/learn-how-to-use-git-and-github-a-beginner-friendly-handbook)

**Do:**
1. One-time setup: `git config --global user.name "Your Name"` and `git config --global user.email "minthawkaung10@gmail.com"` (use the same email as your GitHub account).
2. In `learning/`, run `git init`. Your notes folder is now a repository.
3. The core loop, 5+ times today:
   - change or add a file → `git status` (look every time!) → `git add notes.md` → `git commit -m "describe what changed"`
4. Look at what you built: `git log --oneline`.
5. Undo practice: change a file, *don't* commit, then throw the change away with `git restore <file>`. Also try `git diff` before staging to see exactly what changed.

**Self-check — you can:**
- [ ] Explain the three zones: working folder → staging area (`add`) → history (`commit`)
- [ ] Say why `git status` before every commit is a habit worth having
- [ ] Write a commit message that describes *what changed and why*, not "update"

**Ask Claude:** "Explain the Git staging area with an analogy, then check my understanding."

---

## Day 4 — GitHub: your code in the cloud

**Goal:** connect local Git to GitHub; push and pull.

**Watch/Read:**
- Finish the Traversy crash course from Day 3 (the GitHub part).
- [GitHub official Getting Started docs](https://docs.github.com/en/get-started) — the "Start your journey" section.

**Do:**
1. On github.com, create a **new empty repository** called `learning-journal` (public, no README — you already have files).
2. Connect and push your `learning/` repo to it — GitHub shows you the exact 3 commands on the empty-repo page (`git remote add origin …`, `git branch -M main`, `git push -u origin main`).
3. Refresh the GitHub page — your `notes.md` and its full commit history are there. Click through the commits and see your own history in the UI.
4. Edit `notes.md` **on the GitHub website** (pencil icon), commit there, then on your machine run `git pull`. Watch the change arrive. Now you understand push *and* pull.

**Self-check — you can:**
- [ ] Explain what a "remote" is and what `origin` means
- [ ] Explain the difference between `commit` (local) and `push` (sends to GitHub)
- [ ] Recover when local and GitHub differ (hint: `git pull` first)

**Ask Claude:** "My mental model of push/pull/remote is X — is it right?" (describe it in your own words; I'll correct it.)

---

## Day 5 — Branches and pull requests

**Goal:** the professional workflow — the exact one your portfolio project's CI/CD will be built around.

**Watch/Read:**
- [Learn Git Branching (interactive, in-browser)](https://learngitbranching.js.org/) — do the "Introduction Sequence" (first 4 levels). It's visual and worth an hour.
- [GitHub Skills](https://skills.github.com/) — the "Introduction to GitHub" exercise (hands-on, in a real repo, ~30 min).

**Do — the full professional loop in your `learning-journal` repo:**
1. `git switch -c day5-experiment` (create + switch to a branch)
2. Change `notes.md`, commit.
3. `git push -u origin day5-experiment`
4. On GitHub: open a **Pull Request** from `day5-experiment` into `main`. Read the diff view — green added, red removed.
5. Merge the PR on GitHub, then locally: `git switch main` and `git pull`.
6. Do the whole loop a second time, faster.

**Self-check — you can:**
- [ ] Explain why you branch instead of committing straight to `main`
- [ ] Open, review, and merge a PR
- [ ] Predict what `main` looks like locally after merging on GitHub but before `git pull` (answer: unchanged — that's the point)

**Ask Claude:** "Walk me through a merge conflict on purpose and how to fix one." (We'll create one together safely.)

---

## Day 6 — How the web works: HTTP

**Goal:** understand the conversation your React frontend and Spring Boot backend will have thousands of times.

**Watch/Read:**
- [REST API concepts and examples — WebConcepts (YouTube, ~9 min)](https://www.youtube.com/watch?v=7YcW25PHnAA) — old but still the clearest short intro
- [A Beginner's Guide to REST APIs in Under 10 Minutes (YouTube)](https://www.youtube.com/watch?v=LzOtbUw6f_o)
- [MDN — An overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview) (read the first half)

**Do:**
1. Open your browser's DevTools (F12) → **Network** tab → visit any website. Click a request. Find: the method (GET), the status code (200?), the response. You are watching HTTP happen.
2. From the terminal, make your own requests against a free practice API:
   ```bash
   curl https://jsonplaceholder.typicode.com/posts/1
   curl -i https://jsonplaceholder.typicode.com/posts/9999    # -i shows status: a 404!
   ```
3. Write in your notes, in your own words: what GET, POST, PUT, DELETE each mean, and what 200, 201, 401, 404, 500 each mean. (This table IS your future portfolio API: `GET /api/experience` = read, `POST` = add, `PUT` = update, `DELETE` = remove, `401` = "you're not logged in as admin.")

**Self-check — you can:**
- [ ] Say which HTTP method your portfolio admin panel will use to add / edit / delete an experience
- [ ] Explain what a 401 means and why *your* API will return it a lot (on purpose!)
- [ ] Find any request's method and status code in browser DevTools

**Ask Claude:** "Give me 8 portfolio-website scenarios and let me pick the right HTTP method and expected status code."

---

## Day 7 — JSON + capstone

**Goal:** read and write JSON comfortably, then prove Phase 0 is done with one small capstone.

**Watch/Read:**
- First 30–40 minutes of [APIs for Beginners — freeCodeCamp (YouTube)](https://www.youtube.com/watch?v=GZvSYJDk-us) (the "what is an API / JSON" part)

**Do — Capstone (60–90 min):**
1. In `learning-journal`, create a branch `capstone`.
2. Create a file `my-portfolio-data.json` — hand-write your **real** resume data as JSON:
   ```json
   {
     "summary": "…",
     "education": [
       { "school": "…", "degree": "…", "startYear": 2019, "endYear": 2023 }
     ],
     "workExperience": [
       { "company": "…", "role": "…", "start": "2023-06", "end": null, "description": "…" }
     ],
     "projects": [
       { "name": "…", "description": "…", "technologies": ["…"] }
     ]
   }
   ```
   Validate it by pasting into your browser console as `JSON.parse('…')` or any online JSON validator — fix errors until it parses.
3. Commit, push, open a PR, merge it.
4. Fetch one more thing from the practice API and explain to yourself what came back: `curl https://jsonplaceholder.typicode.com/users/1`

**Why this capstone matters:** that JSON file is almost exactly what your Spring Boot API will send to your React frontend, and its structure is your future MySQL schema. You just designed the heart of your project without knowing it.

**Self-check — Phase 0 complete when:**
- [ ] All Day 1–6 checkboxes are ticked
- [ ] `learning-journal` on GitHub shows ≥10 commits, ≥2 merged PRs
- [ ] Your `my-portfolio-data.json` parses without errors
- [ ] You can explain, in one breath: "the browser sends an HTTP request, the server returns JSON, and Git/GitHub is where the code that does this lives"

**Ask Claude:** "Here's my capstone repo — review my commits, my JSON structure, and tell me if I'm ready for Phase 1." (Paste your JSON and your `git log --oneline` output into the chat.)

---

## Command cheat sheet (print this)

| Command | What it does |
|---|---|
| `pwd` / `ls` / `cd x` / `cd ..` | where am I / list / enter folder / go up |
| `mkdir x` / `touch x` / `rm x` | make folder / make file / delete |
| `cat x` / `nano x` | show file / edit file |
| `git status` | **always look first** |
| `git add x` → `git commit -m "msg"` | stage → save snapshot |
| `git log --oneline` | history |
| `git diff` / `git restore x` | what changed / throw change away |
| `git switch -c name` | new branch |
| `git push` / `git pull` | send to GitHub / get from GitHub |
| `curl -i URL` | make an HTTP request, see status + body |

---

## After Phase 0

Come back and say **"start Phase 1"** — I'll build you the same kind of day-by-day curriculum for HTML/CSS/JavaScript → React. Or at any point during the week: ask me to explain, quiz, or review. Teaching you as you go *is* part of the plan.
