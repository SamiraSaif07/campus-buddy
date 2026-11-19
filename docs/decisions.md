# Decisions

## 2026-10-22 — Staging Dropped and Squash-Merge Adopted (`workflow.md`)
* **Context:** A solo developer cannot have a second reviewer approve PRs, and a separate staging environment adds hosting setup and upkeep with no team to use it.
* **Decision:** Drop the staging environment and keep only `dev` and `production`. Integrate every PR with Squash and Merge after a manual diff review in VS Code for leaked secrets.
* **Rejection:** Rejected a `staging` environment.
* **Consequence:** `main` history has one commit per feature or fix, and production deploys only from Git release tags.

## 2026-11-05 — Re-introduction of Profile Photos via URL Pointers
The photo count (up to 4 mood photos) is superseded by the 2026-11-19 photos entry; the URL-only approach still stands.
* **Context:** The initial project brief marked all image file uploads out-of-scope to adhere to strict zero-cost hosting limitations. However, a platonic matching deck lacks critical visual context and human connection without profile images.
* **Decision:** Re-introduce a main profile photo and up to 4 auxiliary mood photos into the profile suite.
* **Rejection:** Rejected direct binary image file uploads, asset payload handling, and custom cloud file hosting infrastructure.
* **Consequence:** Students will paste a text-string public image link URL into the profile manager. The application stores and renders this strictly as a standard text string data type, preserving a zero-cost infrastructure footprint.

## 2026-11-05 — Consolidation of Daily Cap Reset Logic
* **Context:** The old cap and reset requirements, now N3, featured overlapping mechanisms for managing the daily swipe cap limit.
* **Decision:** Consolidate into a single database runtime constraint. The card query logic counts logs matching the current calendar date stamp. 
* **Rejection:** Rejected a separate daily reset requirement driven by scheduled cron jobs or backend batch reset daemons.
* **Consequence:** N3 is the only cap rule; the count starts again each Asia/Amman calendar date with no reset job.

## 2026-11-05 — Unified Passwordless Sign-In / Registration Gateway
* **Context:** Screen 1 originally only accounted for new user registration onboarding, leaving returning users blocked with no system entry point.
* **Decision:** Consolidate Registration and Sign-in into a single gateway loop on Screen 1. If an email is recognized, the system routes the user straight to Screen 3 upon successful code simulation, bypassing Screen 2.
* **Rejection:** Rejected a separate login screen and password-based sign-in.
* **Consequence:** Eliminates the need to construct password fields, encryption helpers, or independent login screen components.

## 2026-11-05 — Profile Post-Save State Management
* **Context:** A user needs a path to toggle availability or update fields post-save, but multi-tab navigation introduces out-of-scope timeline creep.
* **Decision:** Profile saving is a persistent state override. Screen 3 will include a top navigation button to return to Screen 2 to toggle fields or schedule blocks. Editing profiles after submission is officially in scope via a persistent navigation button.
* **Rejection:** Rejected multi-tab navigation.
* **Consequence:** Screen 3 has a persistent navigation button back to Screen 2, and any edited photo URL re-runs the R3 check.

## 2026-11-05 — Vision Photo Check Stays Simulated
Superseded by the 2026-11-05 server-side URL check entry.
* **Context:** R3 requires every submitted or edited photo URL to pass a Vision validation check before save. A real vision API (e.g., Gemini Flash) costs money per call and needs an API key, which breaks the zero-cost constraint.
* **Decision:** Keep the Vision check simulated: it returns a `pass` or `fail` status and a rejection reason modeled on Gemini Flash, with no paid API call.
* **Rejection:** Rejected integrating a live paid vision API.
* **Consequence:** R3 and R4 (3-strike limiter) stay fully demonstrable at zero cost. Real image moderation is deferred to post-MVP, and Risk 5 (external images changing after save) remains open.

## 2026-11-05 — Buddy Preference Became Required
* **Context:** Buddy Preference (Study, Hangout, or Both) drives the R8 zero-overlap rule.
* **Decision:** Make Buddy Preference a required profile input in R5.
* **Rejection:** Rejected keeping it optional.
* **Consequence:** Every card shows a stated intent, R8 can rely on the field always being present, and the profile cannot save without it.

## 2026-11-05 — Deck Filtering by Landmark or Buddy Preference Killed
* **Context:** The original `brief.md` promised filtering by landmark and buddy preference, but no requirement covered it.
* **Decision:** No deck filters. The deck is ordered only by R7 hierarchical sorting (overlap score, then random tie-breaker); landmark and buddy preference are displayed on the card, not used to filter.
* **Rejection:** Rejected landmark filters, buddy preference filters, and a general filter panel.
* **Consequence:** One query path to build and test. Filtering is listed as out of scope in `brief.md`.

## 2026-11-05 — Social Handles Limited to Instagram and Discord
* **Context:** The profile needed optional social links revealed after a mutual match.
* **Decision:** Collect and display only Instagram and Discord handles, alongside the required WhatsApp number.
* **Rejection:** Killed Twitter (X) and rejected any other social platforms.
* **Consequence:** R5, R11, and N4 cover exactly two optional handles. Other platforms are out of scope in `brief.md`.

## 2026-11-05 — Matches Drawer Added
Superseded by the 2026-11-19 History Buddies screen entry.
* **Context:** The Match Revealed screen (Screen 4) shows contact details only once, at the moment of the match. After leaving it, a user had no way to reach an earlier match again, and nothing was reachable once the daily cap hit.
* **Decision:** Add a toggleable Matches Drawer on Screen 3 (R13) that lists every mutual match's first name with a WhatsApp action button, available even after the daily cap is reached.
* **Rejection:** Rejected a separate Matches screen or tab.
* **Consequence:** Mutual matches must be stored persistently, and N4 releases contact details through both R11 and R13.

## 2026-11-05 — Risk 7 (Shared Mock Code) Accepted
* **Context:** R2 uses a single shared 6-digit mock code for all accounts to avoid building an SMTP verification server. Anyone who knows a peer's ASU email could sign in as them and view their matches' WhatsApp links.
* **Decision:** Accept the risk for the MVP and Demo Night prototype, and record it as Risk 7 in `brief.md`.
* **Rejection:** Rejected building live SMTP email verification within the 8-week window.
* **Consequence:** The sign-in gate is a format check plus a simulated code, not real identity verification. Docs describe it as an ASU-email-gated sign-in, not a verified identity. Real closed-loop verification is post-MVP.

## 2026-11-05 — Name Split into First and Last Name
* **Context:** Cards show only a first name and N4 withholds last names until a mutual match, but the profile stored a single full name.
* **Decision:** Store `first_name` and `last_name` as separate required fields.
* **Rejection:** Rejected deriving the first name from a single full-name field, which breaks on multi-part names.
* **Consequence:** R5 now has seven required inputs. R11 and Screen 4 show first and last name.

## 2026-11-05 — Verification Status Not Stored
* **Context:** A draft data model stored an `is_verified` flag, while R2 holds verification in local frontend state. Every stored account has already passed the shared code, so the flag would always be `true`.
* **Decision:** Remove `is_verified` from the data model.
* **Rejection:** Rejected moving verification status into the database.
* **Consequence:** Verification remains local frontend state only (R2), consistent with Risk 7.

## 2026-11-05 — Birthday Stored as Month and Day Only
* **Context:** R10 needs only the day and month, and N5 forbids displaying a birth year or age.
* **Decision:** Store the optional birthday as `MM-DD`. No year is collected.
* **Rejection:** Rejected storing a full date of birth.
* **Consequence:** The app holds no data from which age can be derived. N5 is satisfied by design.

## 2026-11-05 — WhatsApp Number in International Format
* **Context:** The R11 click-to-open WhatsApp link only works with a number in international format.
* **Decision:** Store the WhatsApp number as digits only in international format (e.g., `9627XXXXXXXX`).
* **Rejection:** Rejected accepting numbers in any format.
* **Consequence:** R5 rejects a save with a malformed number, and every revealed WhatsApp link works.

## 2026-11-05 — Views Logged at Swipe Time, in UTC
* **Context:** If a view were logged when a card is shown, refreshing the page would use up the daily cap.
* **Decision:** Write a `daily_view_logs` row only when a card is swiped, left or right. Store all timestamps in UTC and convert to Asia/Amman only when counting for N3.
* **Rejection:** Rejected logging views when a card is displayed, and rejected storing local-time timestamps.
* **Consequence:** The daily count equals the number of swipes made that Asia/Amman calendar day.

## 2026-11-05 — One Swipe per Profile and a Deck Exhausted State
* **Context:** Without exclusions, the same profiles would return to the deck, and duplicate swipes would break the mutual-match check. With 20 seeded profiles and 15 cards a day, the deck can run out by day two.
* **Decision:** A student can swipe a given profile at most once (N7). The deck excludes the student's own profile and every profile already swiped (R7). When none remain, Screen 3 shows a Deck Exhausted state, separate from Daily Cap Reached.
* **Rejection:** Rejected recycling already-swiped profiles back into the deck.
* **Consequence:** Each pair has at most one swipe in each direction, so a match is exactly two opposite `like` swipes.

## 2026-11-05 — Overlap Score Computed, Not Stored
* **Context:** The deck is sorted by schedule overlap (R7), which depends on two students' current schedules.
* **Decision:** Calculate the Overlap Score at query time from the two `schedule_blocks` arrays.
* **Rejection:** Rejected storing the score as a column.
* **Consequence:** Schedule edits take effect immediately, with no stored score to keep in sync.

## 2026-11-05 — Demo Seed Includes Pre-Liked Swipes
* **Context:** Seeded profiles never swipe on their own, so without seeded likes no mutual match could happen live on Demo Night.
* **Decision:** The N2 seed includes `swipes` rows in which selected mock profiles have already liked the demo account.
* **Rejection:** Rejected a seed with profiles only, which cannot produce a live match.
* **Consequence:** A right-swipe on a pre-liking profile triggers the Match Revealed screen live.

## 2026-11-05 — No Account Deletion
* **Context:** The data model never deletes rows; profile edits overwrite fields in place.
* **Decision:** Students cannot delete their account or remove their phone number in the MVP.
* **Rejection:** Rejected building account deletion or data-removal flows.
* **Consequence:** Simpler for a solo 8-week build. Listed as out of scope in `brief.md`.

## 2026-11-05 — Random, Unguessable Student IDs
* **Context:** The browser session stores the student's `id` in LocalStorage. Sequential IDs such as `STU_00001` would let anyone guess another student's ID and use it as a session.
* **Decision:** Student `id` values are cryptographically random alphanumeric strings with the prefix `STU_`, never sequential.
* **Rejection:** Rejected sequential or predictable IDs.
* **Consequence:** Guessing another student's session is impractical. Copying a known ID into LocalStorage still works, which is accepted under Risk 7.

## 2026-11-05 — Risk 7 Widened to Cover LocalStorage Sessions
* **Context:** The session is the student's `id` stored in browser LocalStorage, so anyone who obtains an ID can paste it in and act as that student, in addition to the shared-code risk already accepted.
* **Decision:** Rename Risk 7 to "Mock Security & LocalStorage Vulnerability" in `brief.md` and accept both holes as a prototype limitation.
* **Rejection:** Rejected building signed or server-issued session tokens within the 8-week window.
* **Consequence:** Random IDs reduce the risk but do not remove it. Real session security is post-MVP.

## 2026-11-05 — Major Is Free Text with Offline Suggestions
* **Context:** A fixed majors list would need to cover every ASU program exactly and would block students whose major is missing.
* **Decision:** Major is an open free-text field. The input offers suggestions from a hardcoded frontend array with no server lookup. R8 compares majors case-insensitively with surrounding whitespace trimmed.
* **Rejection:** Rejected a fixed majors dropdown and rejected server-side suggestions drawn from existing profiles.
* **Consequence:** Spellings that differ beyond case and spacing (e.g., "CS" vs. "Computer Science") do not match in R8. The suggestions steer students toward common spellings.

## 2026-11-05 — "Fifth Year+" Added to Academic Years
* **Context:** Some ASU programs, such as engineering and pharmacy, run longer than four years.
* **Decision:** The Academic Years list is First Year through Fourth Year plus "Fifth Year+".
* **Rejection:** Rejected a four-year-only list.
* **Consequence:** Students in longer programs can state their year accurately.

## 2026-11-05 — Moved Vision Check to Server as URL Format Check
* **Context:** A real image model needs a paid cloud API key, and a check that runs only in the browser can be skipped by overriding a client-side flag from the browser console.
* **Decision:** The simulated Vision check is a server-side check of the URL text only: it must use `https` and end in `.jpg`, `.jpeg`, `.png`, or `.webp`. It never inspects the image and returns a specific rejection reason.
* **Rejection:** Rejected a client-side check, and rejected a real image model.
* **Consequence:** The browser cannot skip the check, but the R4 3-strike lockout stays in the browser and resets on refresh, as accepted in `architecture.md`.

## 2026-11-05 — Stack Chosen
* **Context:** The architecture needed concrete tools that fit a solo 8-week build and a zero-cost budget.
* **Decision:** Vanilla HTML/CSS/JS frontend, Node + Express backend serving the frontend and API from one Render free web service, PostgreSQL on Neon's free plan with a separate Neon project for dev, `pg` with parameterized queries, `dotenv`, and `node --test` for R7 and N3. Render auto-deploy is off; production deploys run through a deploy hook after a Git release tag.
* **Rejection:** Rejected React or another frontend framework, an ORM, MongoDB or Firebase, and Render's free Postgres (expires after 30 days, deleted 14 days later).
* **Consequence:** No build step and one deploy target. The Render cold start is recorded as Risk 8 in `brief.md`. Full details and rejections are in `stack.md`.

## 2026-11-12 — Mock Verification Code Stored in `.env`
* **Context:** R2 uses one shared 6-digit mock code, but no file said where the code lives.
* **Decision:** The code is a server-side `.env` variable, `MOCK_VERIFICATION_CODE`. It is never stored in the database.
* **Rejection:** Rejected storing the code in the database and rejected hard-coding it in source files.
* **Consequence:** `.env.example` lists `MOCK_VERIFICATION_CODE` with no value. The code can differ between dev and production.

## 2026-11-12 — Demo Account Is One of the 20 Seeded Profiles
* **Context:** `data-model.md` referred to "the demo account's `id`" without saying whether it was one of the 20 seeded profiles or an extra row.
* **Decision:** The demo account is one of the 20 seeded Students, `samira.sweidan@students.asu.edu.jo` (changed to `demo.student@students.asu.edu.jo` on 2026-11-19). Other seeded profiles hold the pre-seeded `like` swipes aimed at it.
* **Rejection:** Rejected a 21st profile outside the seed.
* **Consequence:** The database holds exactly 20 profiles, matching N2. The demo account's own deck has 19 candidates.

## 2026-11-12 — Student Row Created on First Save of Screen 2
* **Context:** If the Student row were created on Screen 1, a student who quit before saving their profile would be "recognized" next time (R12) and sent to the deck with an empty profile.
* **Decision:** The Student row and its `id` are created on the first successful save of Screen 2. Until then, the email stays in frontend state and LocalStorage stays empty.
* **Rejection:** Rejected creating the row when the code is verified on Screen 1.
* **Consequence:** Every stored Student has all seven required fields. A student who leaves before saving starts again as a new user.

## 2026-11-12 — Design Choices Accepted (`design.md`)
Two bullets are superseded on 2026-11-19: the Matches button now opens the Buddies screen, and Gender is now required (only MBTI stays blank by default).
* **Context:** Writing `design.md` forced decisions that no earlier file had made.
* **Decision:** Accepted all seven:
  * The Student row is created on the first save of Screen 2 (see the entry above).
  * Screen 3 has a top bar with a Matches button showing a count badge, and an Edit profile button.
  * A rejected duplicate swipe (N7) is skipped silently, with no error shown.
  * A photo URL that fails to load shows a grey box with the student's first-name initial; the rest of the card renders normally.
  * Gender and MBTI default to blank ("Prefer not to say"); MBTI is a picker of the 16 types; interest tags are typed and added with Enter.
  * The schedule grid shows a "N free blocks selected" counter.
  * The on-screen wording for empty, loading and error states is as written in `design.md`.
* **Rejection:** Rejected showing an error for a duplicate swipe, and rejected a broken-image icon in place of the initial fallback.
* **Consequence:** `design.md` is the reference for every screen ticket in `docs/tickets/`.

## 2026-11-19 — Five Screens, with the Card Flip as a State of Screen 3
* **Context:** Students needed more than the card front to decide, and the sitemap drew the extra info as its own page.
* **Decision:** Keep five screens: Gateway, Me, Deck, Match Revealed (modal), History Buddies. The extra info is the back of the same card, shown by an in-place 3D flip on Screen 3 (an instant swap with reduced motion).
* **Rejection:** Rejected a separate profile-details page or route.
* **Consequence:** One card component serves the deck, the Match Revealed modal ("See their card") and the Buddies screen. If week 6 is tight, the animation can drop to an instant swap without breaking R14.

## 2026-11-19 — History Buddies Screen Replaces the Matches Drawer
* **Context:** On 2026-11-05 a drawer was chosen over a separate matches screen. Reviewing a buddy's full profile and contact links does not fit in a drawer.
* **Decision:** Screen 5, History Buddies: a scrollable list, newest first, with photo, name and major on the left and WhatsApp/Instagram/Discord buttons on the right. Tapping a buddy reopens the Match Revealed modal. Reachable from the deck's top bar at any time.
* **Rejection:** Rejected keeping the slide-up Matches Drawer, and rejected a two-column card-and-socials layout (too cramped at phone width).
* **Consequence:** R13 now describes Screen 5. Ticket 09 builds the Buddies screen instead of the drawer.

## 2026-11-19 — Photos: 1 Main + 1 Required Extra + 2 Optional
* **Context:** The card back needs photos, and an empty back would look broken.
* **Decision:** Every profile has a main photo (front) and 1–3 extra photos (back); the first extra is required. Empty slots show a `#EFE9E0` placeholder so the layout keeps its shape. All remain URL strings checked by R3.
* **Rejection:** Rejected up to 4 optional mood photos, and rejected collapsing the layout when slots are empty.
* **Consequence:** `mood_photo_urls` holds 1–3 URLs. R5 counts the first extra photo as required.

## 2026-11-19 — Curated Prompts Instead of Bios; Sharespot Folders Deferred
* **Context:** Shy students need something specific to text about, and open bios invite dating-style self-promotion. Sharespot-style music, book and movie folders would need a media browser.
* **Decision:** Students answer 1–3 prompts from a list of 20 (`prompts.md`), 150 characters each, at least one required. Favourite song, book and movie are prompts in that list. No free-text "About me" field (N8).
* **Rejection:** Rejected a free-text bio, and deferred Sharespot-style media folders to v2.
* **Consequence:** New `prompts` field in `data-model.md`. Prompt answers sit on the card back.

## 2026-11-19 — Gender Required, Hidden from Cards, Same-Gender Default Filter
* **Context:** Campus Buddy must stay platonic without a moderation team.
* **Decision:** Gender is required on Screen 2 but never shown on a card. `gender_preference` defaults to the student's own gender (`Only Girls` / `Only Boys`) and can be switched to `Both`. The filter is mutual: two students see each other only if each fits the other's preference.
* **Rejection:** Rejected an opposite-gender-only option, a one-sided filter, and showing gender as a card badge.
* **Consequence:** New `gender_preference` field and R15. The demo seed needs enough same-gender profiles for the demo account's deck.

## 2026-11-19 — Anti-Dating Guardrails, and a v2 Community Flag
* **Context:** A swipe app on campus can drift into dating.
* **Decision:** Guardrails built into the product: the Screen 1 pledge ("Strictly for platonic study buddies and campus friends at ASU."), the same-gender default, no free-text bios, and cards anchored on schedule overlap, major, year, buddy preference and campus landmarks. Recorded as Risk 9 in `brief.md`.
* **Rejection:** Rejected building reporting or moderation for the MVP.
* **Consequence:** A lightweight 3-strike community flag with auto-suspension is logged as a v2 feature.

## 2026-11-19 — Visual System: Warm Cream, Coral and Sage
* **Context:** The app needed a friendly, campus-oriented look before Figma. Colour inspiration came from Tantan.
* **Decision:** Cream `#F7F3EE` background, white `#FFFFFF` surfaces, coral `#F26B3A` primary accent, sage `#BCE0D3` / `#4BA38A` secondary accent, charcoal `#1E1E1E` text, warm neutral `#EFE9E0` placeholders. Mobile-first at 390–420px, centered column on laptops.
* **Rejection:** Rejected copying any app's logo, mascot or screens; only the palette mood is borrowed.
* **Consequence:** White text on coral is only 3:1, so it is used only for bold labels of 19px or larger. Coral and `#4BA38A` are never body text. Details in `design.md`.

## 2026-11-19 — Demo Account Email Made Non-Personal
* **Context:** The repository is public, so the seed's demo email would be published.
* **Decision:** The demo account is `demo.student@students.asu.edu.jo`, still one of the 20 seeded profiles.
* **Rejection:** Rejected using a real student's address.
* **Consequence:** `data-model.md`, N2 and ticket 03 use the new address.
