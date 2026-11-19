# 03 Demo seed

**Why**
With no real users, the deck and the match flow would be empty on Demo Night. The seed is what makes the demo work. (N2, Risk 3)

**Done when**
* Running the seed script inserts exactly 20 Student rows with random `STU_` ids, each with every required field: a main photo and 1–3 extra photos (public `https` URLs ending in an image extension), 1–3 prompt answers from `prompts.md` (150 characters or fewer), gender, gender preference, buddy preference, major, academic year and schedule blocks.
* One of the 20 is the demo account, `202400000@students.asu.edu.jo`, with `password_hash` made from `SEED_DEMO_PASSWORD`; the other 19 get random passwords. Every seeded email is digits-only before `@students.asu.edu.jo`.
* No plain password appears in the seed script or the repository (N9).
* At least 8 of the other 19 are visible to the demo account under its default same-gender preference (R15).
* At least 3 of those visible profiles have a `like` Swipe (random `SWP_` id) aimed at the demo account.
* At least one visible profile has 0 overlapping blocks with the demo account but shares its major or buddy preference, so R8 can be shown.
* At least one visible profile has only 1 extra photo and 1 prompt, so the card-back placeholders can be shown.
* Running the seed twice still leaves exactly 20 Students and no duplicate Swipes.

**Touches**
`/seed` script, dev Neon database
