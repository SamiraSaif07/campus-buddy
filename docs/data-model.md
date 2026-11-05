# Data Model

Three nouns: Student, View Log, Swipe. A Match is not stored; it is derived from two Swipes.

## Student (`students`)
* `id`: cryptographically random, unguessable alphanumeric string with the prefix `STU_` (e.g., `STU_XXXXX`). Never sequential.
* `email`: unique; must end with `@students.asu.edu.jo`. Used for the R12 returning-user lookup.
* `first_name`: required. Shown on cards.
* `last_name`: required. Withheld until a mutual match (N4).
* `major`: required; open free text. Compared case-insensitively with surrounding whitespace trimmed (R8). The input suggests values from the Major Suggestions list but accepts any text.
* `academic_year`: required; one value from the Academic Years list.
* `buddy_preference`: required; `Study`, `Hangout`, or `Both`.
* `whatsapp_number`: required; digits only, international format (e.g., `9627XXXXXXXX`). Withheld until a mutual match (N4).
* `main_photo_url`: required; text URL to a publicly hosted image.
* `mood_photo_urls`: optional; up to 4 text URLs.
* `gender`: optional; one value from the Gender Options list.
* `mbti`: optional; four-letter type.
* `birthday`: optional; month and day only, as `MM-DD`. No year is stored.
* `interest_tags`: optional; up to 5 open free-text tags.
* `preferred_landmark`: optional; one value from the ASU Landmarks list.
* `instagram_handle`: optional. Withheld until a mutual match (N4).
* `discord_handle`: optional. Withheld until a mutual match (N4).
* `schedule_blocks`: 25 booleans, one per cell of the 5×5 grid (Sunday–Thursday × 5 blocks).
* `created_at`: UTC timestamp.

## View Log (`daily_view_logs`)
* `id`: cryptographically random alphanumeric string with the prefix `LOG_`.
* `user_id`: the Student who swiped.
* `viewed_profile_id`: the Student whose card was swiped.
* `viewed_at`: UTC timestamp of the swipe.

## Swipe (`swipes`)
* `id`: cryptographically random alphanumeric string with the prefix `SWP_`.
* `swiper_id`: the Student who swiped.
* `target_id`: the Student whose card was swiped.
* `direction`: `like` or `pass`.
* `created_at`: UTC timestamp.

## Relationships
* A Student has zero or more View Logs and zero or more Swipes, as swiper and as target.
* A Match exists between Student A and Student B when a `like` Swipe exists from A to B and a `like` Swipe exists from B to A. The Matches Drawer (R13) and the Match Revealed screen (R11) both read Matches this way.

## Rules
* A View Log row is written at the moment a card is swiped, left or right. Showing a card writes nothing.
* A Student can swipe a given target at most once: one Swipe per (`swiper_id`, `target_id`) pair.
* The deck for a Student excludes their own `id` and every `target_id` they have already swiped. When no candidates remain, the deck shows the Deck Exhausted state.
* All timestamps are stored in UTC. The N3 daily cap converts to the Asia/Amman calendar date only when counting.
* Overlap Score is calculated at query time by counting cells that are `true` in both Students' `schedule_blocks`. It is never stored.
* No rows are ever deleted. Profile edits overwrite the Student's fields in place.

## Not stored
* Verification status: local frontend state only (R2).
* Photo validation attempts and the 15-minute lockout: local frontend session only (R4).
* Session: held in browser LocalStorage as `{"asu_matcher_student_id": "STU_XXXXX"}`. The browser stores the student's `id` directly for passwordless persistence.
* Overlap Score: computed at query time.
* Matches: derived from Swipes.

## Fixed lists
Kept as static configuration arrays in one shared place, so new universities can add their own later (Risk 6).
* Academic Years: ['First Year', 'Second Year', 'Third Year', 'Fourth Year', 'Fifth Year+']
* Gender Options: ['Male', 'Female']
* ASU Landmarks: ['IT Faculty Lab', '360 Square', 'Main Library', 'Deanship Courtyard', 'Engineering Building Cafe']
* Major Suggestions (frontend only, offline; suggestions, not a restriction): ['Computer Science', 'Software Engineering', 'Cybersecurity', 'Artificial Intelligence', 'Computer Information Systems', 'Civil Engineering', 'Electrical Engineering', 'Mechanical Engineering', 'Architecture', 'Pharmacy', 'Nursing', 'Accounting', 'Business Administration', 'Marketing', 'Law', 'English Language and Literature', 'Graphic Design', 'Interior Design']

## Demo seed (N2)
* 20 Student rows covering diverse ASU majors, interest tags, schedule blocks, landmarks, and public photo URLs.
* Seeded Swipe rows in which selected mock profiles have already sent a `like` to the demo account's `id`, so a right-swipe on one of them produces a mutual match live on Demo Night.