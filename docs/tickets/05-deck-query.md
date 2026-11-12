# 05 Deck query

**Why**
The deck order is the product: students with the most shared free blocks come first. (R7, R8, N3)

**Done when**
* The deck route returns candidates for the signed-in student, excluding their own profile and every profile they have already swiped.
* Candidates are sorted by Overlap Score (cells `true` in both `schedule_blocks`), highest first, with a random tie-breaker; the score is returned with each card and never stored.
* Each card says whether the R8 zero-overlap notice applies, comparing majors case-insensitively with whitespace trimmed.
* The response contains no last name, WhatsApp number, Instagram or Discord handle (N4).
* When the student has 15 or more view logs on today's Asia/Amman date, the route returns a "cap reached" signal instead of cards; when no candidates remain, it returns a "deck exhausted" signal.
* `node --test` covers: the highest-overlap profile is at index 0 on every run; own and already-swiped profiles never appear; the cap signal at 15 logs.

**Touches**
Deck route in `/server/routes`, `students`, `swipes`, `daily_view_logs` (read only), `/tests`