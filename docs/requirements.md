# Requirements

## Must do

### R1 — ASU Email Domain Restriction & Community Pledge (Screen 1)
* **Specification:** Sign-up and log-in accept only ASU student emails: digits only before `@students.asu.edu.jo` (the student number, e.g. `202400000@students.asu.edu.jo`); letters, dots and any other suffix are rejected. The check runs on the server; the browser repeats it only to show the error sooner. The welcome view and the sign-up view display the pledge line: "Strictly for platonic study buddies and campus friends at ASU."
* **Done when:** An email with letters or dots before the `@`, or any other suffix, shows "Use your ASU student email, e.g. 202400000@students.asu.edu.jo." and goes no further, and the pledge line is visible on the welcome and sign-up views.

### R2 — Simulated Code Verification at Sign-Up (Screen 1)
* **Specification:** After the sign-up email passes R1, is not already registered, and the Campus Buddy password has at least 8 characters, the code view asks for a 6-digit code. All accounts use a single shared mock code (`MOCK_VERIFICATION_CODE`). Verification status is held in local frontend state and is not stored. "Resend code" sends nothing in the MVP and says so honestly. Log-in never asks for a code.
* **Done when:** The correct code shows the Verified view ("You're in, ASUer. Let's set up your profile.") and routes to Screen 2; any other value shows "That code is not correct. Try again." and stays on the code view.

### R3 — Profile Photo URL Validation (Screen 2)
* **Specification:** Every profile photo (the main photo and each extra photo) is submitted as a text URL string pointing to a publicly hosted image. Each submitted URL is passed to a simulated Vision check run on the server (a URL format check; see decisions.md). If the check returns `fail`, the app keeps the user on Screen 2, displays the exact rejection reason returned by the check, and blocks save actions until the URL passes. Any later edit to a saved photo URL re-triggers the check, blocking save actions until a status of `pass` is returned.
* **Done when:** A user with any photo URL marked `fail` cannot save the profile or advance to the swipe deck, and editing a saved photo URL to one marked `fail` blocks the save.

### R4 — Photo Validation Attempt Limiter (Screen 2)
* **Specification:** A local frontend session cap allows exactly 3 consecutive failed photo URL validation attempts per active session.
* **Done when:** On the 3rd consecutive failed check, photo URL submission is disabled for exactly 15 minutes, a live countdown timer is displayed, and a campus compliance message is shown.

### R5 — Mandatory & Optional Profile Data (Screen 2)
* **Specification:**
  * Required inputs (11): First Name, Last Name, Major, Academic Year, Gender, Gender Preference (R15), Buddy Preference (Study, Hangout, or Both), WhatsApp number, main photo URL, at least 1 extra photo URL, and at least 1 prompt answer.
  * Optional inputs: up to 2 more extra photo URLs (3 extras in total), up to 2 more prompt answers (3 in total), MBTI, Birthday (month and day only), up to 5 interest tags, preferred ASU landmark, Instagram handle, Discord handle.
  * Prompts are chosen from the 20 in `prompts.md`; each prompt may be used once per profile, and each answer is at most 150 characters.
  * The WhatsApp number is stored as digits only in international format (e.g., `9627XXXXXXXX`).
  * All photos are stored as text URL strings only.
* **Done when:** The profile saves if and only if all 11 required inputs are present, the WhatsApp number is digits-only international format, and every prompt answer is 150 characters or fewer; a save attempt failing any condition is rejected.

### R6 — 5×5 Timetable Schedule Grid (Screen 2)
* **Specification:** Students set availability by toggling a 5×5 grid: 5 daily blocks (8:30–10:00, 10:00–11:30, 11:30–1:00, 1:00–2:30, 2:30–4:00) across 5 campus days (Sunday through Thursday).
* **Done when:** The saved selections map to a 25-element boolean schedule array in the data layer, with each element matching its grid cell.

### R7 — Deck Selection & Hierarchical Sorting (Screen 3)
* **Specification:** When building the daily deck, the backend excludes the user's own profile, every profile the user has already swiped, and every profile ruled out by the R15 gender filter. It sorts the remaining candidates primarily by number of overlapping timetable blocks with the user (descending), calculated at query time, and secondarily by a random tie-breaker. When no candidates remain, Screen 3 shows the Deck Exhausted state.
* **Done when:** For a test dataset with a known highest-overlap profile, the query returns that profile at index 0 on every run; the user's own profile, already-swiped profiles and gender-filtered profiles never appear; and a user with no remaining candidates sees the Deck Exhausted state, not the Daily Cap Reached screen.

### R8 — Zero-Overlap Card Treatment (Screen 3)
* **Specification:** Profiles that share the user's Major or Buddy Preference but have 0 overlapping schedule blocks remain in the deck. They display the notice: "0 Overlapping Windows — Schedules conflict, but you share a major or buddy preference!" Major string validations must execute via case-insensitive, whitespace-trimmed comparison filters.
* **Done when:** A zero-overlap profile with a shared Major or Buddy Preference renders in the swipe deck with the notice visible.

### R9 — Card Front Contents (Screen 3)
* **Specification:** The front of the swipe card displays: main photo, First Name, Overlap Score badge (or the R8 notice), Major, Academic Year, Buddy Preference, and the R10 birthday tag when it applies. Gender is never displayed on any card.
* **Done when:** Every listed field renders on the front of the card, no other profile field appears there, and no card shows gender.

### R10 — Birthday Tag Visibility (Screen 3)
* **Specification:** If a profile's stored birthday (`MM-DD`) matches the current Asia/Amman calendar day, the tag "🍰 B-DAY TODAY!!" renders next to the first name on the card front.
* **Done when:** The tag appears on profiles whose birthday matches today's date and on no others.

### R11 — Match Revealed Modal (Screen 4)
* **Specification:** When a user's right-swipe completes a mutual right-swipe, a modal opens over Screen 3 showing: main photo, First Name, Last Name, Major, Academic Year, Instagram handle, Discord handle, a click-to-open WhatsApp link, and a "See their card" control that shows the R14 card back. The same modal reopens when the user taps a buddy on Screen 5.
* **Done when:** A mutual right-swipe immediately opens the modal with the listed fields and a working WhatsApp link, and tapping a buddy on Screen 5 opens the same modal for that buddy.

### R12 — Separate Sign-Up and Log-In with a Campus Buddy Password (Screen 1)
* **Specification:** Sign-up and log-in are separate views. Sign-up takes the email and a Campus Buddy password (at least 8 characters, unrelated to the ASU portal password); if the email already has a Student row, sign-up stops with "This email already has an account." and a link to log in. The password stays in frontend state until the first Save Profile on Screen 2, when the server stores it only as `password_hash` (N9). Log-in takes the email and password; the server compares the password with `password_hash` and, on a match, returns the student's `id`, which is saved to LocalStorage before routing to Screen 3.
* **Done when:** A returning student logs in with email and password, skips the code view and Screen 2, and lands on Screen 3 with their profile intact; an unknown email and a wrong password both show "Incorrect email or password."; signing up with a registered email never creates a second account.

### R13 — Buddies Screen (Screen 5)
* **Specification:** The system stores every swipe persistently. Mutual matches are derived from two `like` swipes in opposite directions and are listed on Screen 5, newest match first. Each row shows the buddy's main photo, full name and major on the left and WhatsApp, Instagram and Discord buttons on the right (Instagram and Discord only if present). Screen 5 is reachable from Screen 3 at any time, including in the Daily Cap Reached and Deck Exhausted states.
* **Done when:** After a mutual match, reopening Screen 5 lists that buddy with a working WhatsApp button, and the list survives a page reload and a new sign-in.

### R14 — Card Flip to Profile Back (Screen 3)
* **Specification:** A visible flip button on the card turns it over in place (3D rotation; an instant swap when the user has reduced motion enabled). The back shows the 3 extra photo slots down the left side (empty slots shown as warm-neutral placeholders), the 1–3 prompt questions with their answers beside the photos, interest tags, MBTI, and preferred ASU landmark. Pass and Like work from either side; the flip button returns to the front.
* **Done when:** Tapping flip shows the back with the listed content and no contact details; tapping flip again shows the front; and a profile with 1 extra photo shows 2 placeholder slots.

### R15 — Same-Gender Default Filter (Screens 2 & 3)
* **Specification:** On Screen 2, each student chooses a Gender Preference of either their own gender only (`Only Girls` or `Only Boys`) or `Both`. It defaults to their own gender. An opposite-gender-only option is not offered. The deck filter is mutual: A sees B only if B's gender fits A's preference and A's gender fits B's preference.
* **Done when:** A new profile saves with the same-gender preference unless `Both` is chosen; a student with the same-gender preference never sees an opposite-gender card; and a `Both` student never sees a student whose preference excludes them.

## Must never do

### N1 — Never Run In-App Messaging
* **Specification:** The system never maintains real-time chat pipelines, in-app message boxes, WebSocket listeners, or user-to-user data relays. All communication is offloaded to WhatsApp links.
* **Done when:** The repository contains zero messaging controllers, socket listeners, or chat hooks.

### N2 — Never Depend on Live Registrations for Demo Night
* **Specification:** Persistent storage is pre-populated with exactly 20 mock ASU student profiles, one of which is the demo account (`202400000@students.asu.edu.jo`), covering diverse majors, interest tags, schedule blocks, landmarks, public photo URLs and prompt answers. The seed also includes swipes in which selected profiles visible to the demo account under its gender preference have already liked it.
* **Done when:** A fresh login as the demo account on Demo Night loads a deck with overlap scores and working swipe mechanics without any live user registrations, and a right-swipe on a pre-liking profile opens the Match Revealed modal.

### N3 — Never Serve More Than 15 Cards per Day
* **Specification:** Each swipe, left or right, writes a record to `daily_view_logs` (user_id, viewed_profile_id, viewed_at) at the moment of the swipe. Timestamps are stored in UTC. Before serving cards, the backend counts the user's records whose `viewed_at`, converted to Asia/Amman, falls on the current calendar date. If the count is greater than or equal to 15, card retrieval is blocked.
* **Done when:** At a count of 15, the app returns an empty stack and displays the "Daily Cap Reached" screen; on the next Asia/Amman calendar date, the same user receives cards again with no separate reset job.

### N4 — Never Expose Contact Details Before a Mutual Match
* **Specification:** Last names, WhatsApp numbers, Instagram handles, and Discord handles are withheld at the database layer from every response that serves the swipe deck cards (front and back), and are released only for mutual matches through the R11 modal and the R13 Buddies screen.
* **Done when:** Inspecting the network payload for deck cards shows no last names, phone numbers, Instagram handles, or Discord handles, and a one-sided right-swipe reveals none of them.

### N5 — Never Display a Birthday or Age
* **Specification:** No screen displays a user's birthday date or age. Birth years are never collected.
* **Done when:** No rendered card or screen contains a birthday date or age value for any profile.

### N6 — Never Upload, Store, or Host Image Files
* **Specification:** Campus Buddy stores photos only as text URL strings pointing to public images. No image file is accepted, stored, or served by the application.
* **Done when:** The application has no file upload input, and the database contains no image binary data.

### N7 — Never Swipe the Same Profile Twice
* **Specification:** A student can swipe a given target profile at most once.
* **Done when:** A second swipe attempt by the same student on the same target is rejected and creates no new record.

### N8 — Never Offer a Free-Text Bio
* **Specification:** There is no open "About me" field. Self-description is limited to 1–3 prompt answers from `prompts.md` (150 characters each) and up to 5 interest tags.
* **Done when:** Screen 2 contains no free-text field other than names, major, interest tags, handles, photo URLs and prompt answers, and the server rejects any prompt answer over 150 characters or any prompt not in `prompts.md`.

### N9 — Never Store or Return a Plain Password
* **Specification:** The Campus Buddy password is hashed on the server with Node's built-in `crypto.scrypt` and a random salt, and only `password_hash` is stored. The plain password is never written to the database, logs, LocalStorage or the repository, and `password_hash` is never sent to the browser.
* **Done when:** The `students` table holds no readable password, no API response contains `password_hash`, and log-in still works after a server restart.
