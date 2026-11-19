# 05 Deck query

**Why**
The deck order is the product: students with the most shared free blocks come first, and only students who fit each other's gender preference ever meet. (R7, R8, R15, N3, N4)

**Done when**
* The deck route returns candidates for the signed-in student, excluding their own profile, every profile they have already swiped, and every profile ruled out by the mutual gender filter (each must fit the other's `gender_preference`).
* Candidates are sorted by Overlap Score (cells `true` in both `schedule_blocks`), highest first, with a random tie-breaker; the score is returned with each card and never stored.
* Each card says whether the R8 zero-overlap notice applies, comparing majors case-insensitively with whitespace trimmed.
* Each card includes its front fields and its back fields (extra photos, prompts, interest tags, MBTI, landmark), and no last name, WhatsApp number, Instagram, Discord, gender or gender preference.
* When the student has 15 or more view logs on today's Asia/Amman date, the route returns a "cap reached" signal instead of cards; when no candidates remain, it returns a "deck exhausted" signal.
* `node --test` covers: the highest-overlap profile is at index 0 on every run; own, already-swiped and gender-filtered profiles never appear; a `Both` student never sees someone whose preference excludes them; the cap signal at 15 logs.

**Touches**
Deck route in `/server/routes`, `students`, `swipes`, `daily_view_logs` (read only), `/tests`
