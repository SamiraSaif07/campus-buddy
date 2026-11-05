# Coding Rules

This framework governs all development processes, file structure configurations, naming rules, and script generation tasks performed inside this repository over our 8-week cycle.

## 1. Golden AI Collaboration Rule
* **If any functional parameter, edge case, layout condition, or requirement is ambiguous, you must stop immediately and ask me for clarification in chat instead of deciding it yourself.**

## 2. Directory & Repository File Layout Architecture
All code files must live strictly within the following clean decoupled layout map:
* `/public/` — Frontend client viewport assets (`index.html`, `styles.css`, `app.js`).
* `/server/db/` — Native database driver setup and `pg` connection pool configuration files.
* `/server/routes/` — Express route handler scripts (exactly one route file corresponding to each functional screen).
* `/seed/` — Persistent database initial static profile pre-seeding scripts.
* `/tests/` — Automated R7 and N3 test files.

## 3. Strict Naming & Casing Conventions
* File and Folder Names: Lowercase kebab-case format only (`user-profile-cards.js`).
* Frontend and Backend Script Variables/Functions: Lowercase camelCase format only (`calculateOverlapScore`).
* Database Tables and Column Attributes: Lowercase snake_case format only (`whatsapp_number`, `viewed_profile_id`).

## 4. Code Construction, Comments, & Error Handling
* **Code Style:** Write clean, vanilla, asynchronous ES6 JavaScript for both frontend elements and Express routes. Framework builders, compilers, or packers are banned.
* **Comment Policy:** Comments must explain *why* a particular piece of logic or architecture constraint exists, *never what* the script is line-by-line doing. Avoid redundant code commentary.
* **Error Handling Mandate:** Never swallow, catch-and-ignore, or silently pass an error. Every catch block must log the failure to the console and return a clear, structured JSON error response status payload to the client browser view.

## 5. Testing Scope & Constraints
* Automated tests driven natively via `node --test` must execute locally to validate database script behavior before Pull Request reviews.
* Mandatory Automated Test Coverage:
  * Requirement R7: The hierarchical database sorting query routine (overlap score descending, then random tie-breaker).
  * Requirement N3: The 15-card daily cap log query calculation.
* Frontend layout validations, forms, and views are verified by hand at a mobile screen layout width; they do not use automated browser scripts.

## 6. Absolute System Directives ("The Never List")
* **Never** install any third-party npm package, library, or dependency without explicitly asking for permission in chat first.
* **Never** modify, alter, or touch code files outside the boundary of the specific feature ticket you are actively assigned to.
* **Never** push source code changes directly to the stable `main` branch or merge your own Pull Requests.
* **Never** commit, upload, or stage an environment variable `.env` file containing database passwords, ports, or secrets into the repository history.
* **Never** build SQL by joining strings with user input.

## 7. Data Handling Rules
* Every query uses `pg` parameterized placeholders (`$1`, `$2`, …) through the connection pool. Never build SQL by joining strings with user input.
* Major string validations inside matching engines must utilize case-insensitive, whitespace-trimmed comparison filters.
* All operational timestamp properties must be generated and stored natively in UTC format, converting to Asia/Amman timezone constraints only when counting the daily cap and checking birthdays.