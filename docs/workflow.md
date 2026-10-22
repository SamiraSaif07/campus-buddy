# How We Work

## 1. Team & Project Bounding
* **The Team:** A solitary developer managing all engineering tasks.
* **The AI Collaborator (Claude Pro):** Acts strictly as a junior coding partner. Claude is permitted to write features on `feature/*` branches, edit documentation files, and generate code snippets. Claude is strictly forbidden from pushing directly to `main` or merging its own Pull Requests.
* **Definition of Done:** A ticket or feature is considered "Done" only when it builds locally without console errors, passes responsive styling inspection at a simulated mobile layout width, is committed with a verified semantic message, and is successfully integrated into the stable `main` branch.

## 2. Branching Strategy
* `main`: Always deployable, stable, and running. Never commit code directly to `main`. *Exception:* Setting up the initial repository structure, updating the top-level project `README.md`, or patching immediate text layout bugs in `/docs` markdown files is allowed directly on `main`.
* `feature/*`: One branch per structural feature milestone (e.g., `feature/01-user-cards`, `feature/02-schedule-blocks`). 
* `fix/*`: One branch per verified codebase bug fix (e.g., `fix/03-email-domain-check`).
* **Branch Mechanics:** All branches must branch directly from the latest pulled version of `main`. Branch naming slugs must be entirely lowercase, use alphanumeric characters, and use hyphens for spacing (`kebab-case`).

## 3. Commit Message Formatting
* **Strict Format Structure:** `type(scope): description in lowercase imperative mood` (e.g., `feat(auth): add email registration input string`).
* **Commit Types Permitted:**
  * `feat`: A brand new feature or visual interface element.
  * `fix`: A resolution to a broken functional code routine or logic bug.
  * `docs`: Additions or modifications to the files inside `/docs` or the top `README.md`.
  * `refactor`: Structural optimization or renaming code files with no behavior modifications.
  * `test`: Adding or altering fake database seeds or verification simulation logic.
  * `chore`: Updating `.gitignore`, altering configuration parameters, or adjusting package dependencies.
* **Banned Messaging Syntax:** Never use open-ended keywords like `"update"`, `"changes"`, `"wip"`, or temporary placeholders like `"fix stuff"`. Every message must detail exactly what changed in plain words.

## 4. Pull Request Integration (The Solo Code Review Protocol)
* **The Structure:** Exactly one feature branch corresponds to one active Pull Request. Pull Request descriptions must cleanly summarize what changed, the specific verification steps to manually test the feature, and a note indicating mobile view layout rendering is verified.
* **The Solo Approval Process:** To resolve the reviewer paradox, the developer will utilize a Squash and Merge strategy on GitHub. Before pressing merge, the developer must manually review the file diff changes inside VS Code to verify no API keys, passwords, or raw configuration variables are leaked. 
* **Branch Cleanup:** Once a Pull Request is successfully squashed and integrated into `main`, the local and remote branch must be permanently deleted immediately to prevent branch accumulation.

## 5. Environment Boundaries & Data Routing
* `dev`: Local runtime workspace executing inside VS Code.
  * **Data:** Powered by a local data file schema or a free local database instance populated with a subset of fake profiles.
  * **Rule:** Local runtime variables must never reference production endpoints or point to live operational infrastructure assets.
* `production`: The live web app architecture accessible via your public demo link.
  * **Data:** Seeded exclusively with the 20 structured mock student profiles modeling realistic ASU computer science majors, class blocks, and landmark selections to guarantee functional swiping on Demo Night.
  * **Secrets Management:** Sensitive identifiers, local port metrics, and API variables must live securely inside an isolated local `.env` configuration file. The actual `.env` file is explicitly added to the `.gitignore` asset file to guarantee it is never committed to public GitHub history. A harmless, value-free `.env.example` template file will be committed to the repository to track necessary keys.
  * **Deployment Trigger:** Deployments to production occur strictly through explicit Git Release Tags (e.g., merging code to `main` accumulates verified stability, and pushing a tag like `v0.2.0` updates the public live deployment link).