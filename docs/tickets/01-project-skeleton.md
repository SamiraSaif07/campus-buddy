# 01 Project skeleton

**Why**
Every other ticket needs a running server, a database connection and a test runner before it can be built or checked. (stack.md, rules.md)

**Done when**
* `npm start` runs an Express server that serves `/public/index.html` at `/` and shows "Campus Buddy".
* The server connects to the dev Neon database through a `pg` pool using `DATABASE_URL` from `.env`, and logs one line confirming the connection.
* `.env` is listed in `.gitignore`; `.env.example` is committed with `DATABASE_URL`, `MOCK_VERIFICATION_CODE` and `SEED_DEMO_PASSWORD` and no values.
* `node --test` runs and passes one placeholder test.
* The folders `/public`, `/server/db`, `/server/routes`, `/seed` and `/tests` exist, as rules.md requires.

**Touches**
`package.json`, `.gitignore`, `.env.example`, `/server` entry file, `/server/db` pool file, `/public/index.html`, `/tests`
