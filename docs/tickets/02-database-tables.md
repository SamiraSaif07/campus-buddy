# 02 Database tables

**Why**
The deck, swipes, matches and daily cap all read and write these three tables; they must exist exactly as data-model.md describes. (data-model.md, N3, N7)

**Done when**
* Running the table script against the dev database creates `students`, `daily_view_logs` and `swipes` with every field and type listed in data-model.md.
* `students.email` is unique.
* `swipes` rejects a second row with the same (`swiper_id`, `target_id`) pair at the database level.
* All timestamp columns store UTC.
* Running the script twice does not fail or duplicate anything.

**Touches**
`/server/db` table script, dev Neon database
