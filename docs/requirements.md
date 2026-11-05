# Requirements

## Must do

### R1 — ASU Email Domain Restriction (Screen 1)
* **Specification:** The registration interface validates that the email input ends exactly with the `@students.asu.edu.jo` suffix.
* **Done when:** An email ending in any other suffix displays an error banner and no account is created.

### R2 — Simulated Registration String Code Verification (Screen 1)
* **Specification:** After the email passes R1, the account enters the Pending State UI view and prompts the user to enter a 6-digit mock registration code as a text string. Verification status is held in local frontend state. All accounts use a single shared mock code.
* **Done when:** Entering the correct mock code switches the account from the Pending State UI view to the Verified State UI view and routes a new user to Screen 2 (returning users are routed by R12); entering any other value keeps the user in the Pending State UI view. 

### R3 — Profile Photo URL Validation (Screen 2)
* **Specification:** Profile photos are submitted as text URL strings pointing to publicly hosted images. Each submitted URL is passed to a simulated Vision validation check (modeled on Gemini Flash). If the check returns `fail`, the app keeps the user on Screen 2, displays the exact rejection reason returned by the check, and blocks save actions until a URL passes. Any subsequent update, modification, or edit to a saved profile photo URL must instantly re-trigger the simulated Vision validation check, blocking save actions until a status of `pass` is returned.
* **Done when:** A user whose main photo URL is marked `fail` cannot save the profile or advance to the swipe deck, and editing a saved photo URL to one marked `fail` blocks the save.

### R4 — Photo Validation Attempt Limiter (Screen 2)
* **Specification:** A local frontend session cap allows exactly 3 consecutive failed photo URL validation attempts per active session.
* **Done when:** On the 3rd consecutive failed check, photo URL submission is disabled for exactly 15 minutes, a live countdown timer is displayed, and a campus compliance message is shown.

### R5 — Mandatory & Optional Profile Data (Screen 2)
* **Specification:**
  * Required inputs: Full Name, Major, Academic Year, Buddy Preference (Study, Hangout, or Both), WhatsApp number, and main photo URL.
  * Optional inputs: Gender, MBTI, Birthdate, up to 5 interest tags, preferred ASU landmark, Instagram handle, Discord handle, and up to 4 mood photo URLs.
  * All photos are stored as text URL strings only.
* **Done when:** The profile saves if and only if all six required inputs are present; a save attempt with any required input missing is rejected.

### R6 — 5×5 Timetable Schedule Grid (Screen 2)
* **Specification:** Students set availability by toggling a 5×5 grid: 5 daily blocks (8:30–10:00, 10:00–11:30, 11:30–1:00, 1:00–2:30, 2:30–4:00) across 5 campus days (Sunday through Thursday).
* **Done when:** The saved selections map to a 25-element boolean schedule array in the data layer, with each element matching its grid cell.

### R7 — Hierarchical Deck Sorting (Screen 3)
* **Specification:** When building the daily deck, the backend sorts candidate profiles primarily by number of overlapping timetable blocks with the user (descending) and secondarily by a random tie-breaker.
* **Done when:** For a test dataset with a known highest-overlap profile, the query returns that profile at index 0 on every run.

### R8 — Zero-Overlap Card Treatment (Screen 3)
* **Specification:** Profiles that share the user's Major or Buddy Preference but have 0 overlapping schedule blocks remain in the deck. They display the notice: "0 Overlapping Windows — Schedules conflict, but you share a major or buddy preference!"
* **Done when:** A zero-overlap profile with a shared Major or Buddy Preference renders in the swipe deck with the notice visible.

### R9 — Public Swipe Card Contents (Screen 3)
* **Specification:** The public swipe card displays: First Name, main photo, mood photos, Major, Academic Year, Gender, Buddy Preference, interest tags, MBTI, preferred ASU landmark, and Overlap Score.
* **Done when:** Every field listed renders on the card when present in the profile, and absent optional fields render no empty placeholder.

### R10 — Birthday Tag Visibility (Screen 3)
* **Specification:** If a profile's birthdate matches the current calendar day, the tag "🍰 B-DAY TODAY!!" renders next to the first name.
* **Done when:** The tag appears on profiles whose birthdate matches today's date and on no others.

### R11 — Match Revealed Gateway (Screen 4)
* **Specification:** When a user's right-swipe completes a mutual right-swipe, the app transitions to Screen 4 and displays: Full Name, Major, Academic Year, Instagram handle, Discord handle, and a click-to-open WhatsApp link.
* **Done when:** A mutual right-swipe immediately displays the listed fields and a working WhatsApp link.

### R12 — Unified Gateway Checking Routine (Screen 1)
* **Specification:** When an email input is processed on Screen 1, the backend performs a unique record lookup query. If the email is recognized, entering the valid 6-digit mock code re-initializes the user's persistent local session and routes the viewport directly to Screen 3, bypassing Screen 2 profile setup. If the email is not recognized, the R2 new-user path applies.
* **Done when:** A returning user completes code verification, skips Screen 2 entirely, and lands on Screen 3 with their existing profile data intact; no duplicate account is created for a recognized email.

### R13 — Persistent Match Storage (Screen 3)
* **Specification:** The system stores all mutual right-swipe connections. Match contact credentials remain persistently accessible to the user via the Screen 3 Matches Drawer.
* **Done when:** Re-opening the deck view displays a persistent row of all previous mutual matches, each with its First Name and a working WhatsApp action button.

## Must never do

### N1 — Never Run In-App Messaging
* **Specification:** The system never maintains real-time chat pipelines, in-app message boxes, WebSocket listeners, or user-to-user data relays. All communication is offloaded to WhatsApp links.
* **Done when:** The repository contains zero messaging controllers, socket listeners, or chat hooks.

### N2 — Never Depend on Live Registrations for Demo Night
* **Specification:** Persistent storage is pre-populated with exactly 20 mock ASU student profiles covering diverse majors, interest tags, schedule blocks, landmarks, and public photo URLs.
* **Done when:** A fresh login on Demo Night loads a deck with overlap scores and working swipe mechanics without any live user registrations.

### N3 — Never Serve More Than 15 Cards per Day
* **Specification:** Each card view is recorded in `daily_view_logs` (user_id, viewed_profile_id, viewed_at). Before serving cards, the database counts records matching the user's ID whose `viewed_at` falls on the current calendar date (Asia/Amman). If the count is greater than or equal to 15, card retrieval is blocked.
* **Done when:** At a count of 15, the app returns an empty stack and displays the "Daily Cap Reached" screen; on the next calendar date, the same user receives cards again with no separate reset job.

### N4 — Never Expose Contact Details Before a Mutual Match
* **Specification:** Last names, WhatsApp numbers, Instagram handles, and Discord handles are withheld at the database layer from every response that serves the swipe deck cards, and are released only for mutual matches through the R11 Match Revealed screen and the R13 Matches Drawer.
* **Done when:** Inspecting the network payload for swipe deck cards shows no last names, phone numbers, Instagram handles, or Discord handles, and a one-sided right-swipe reveals none of them.

### N5 — Never Display a Birthdate or Age
* **Specification:** No screen displays a user's numerical birthdate, birth year, or age.
* **Done when:** No rendered card or screen contains a birthdate, birth year, or age value for any profile.

### N6 — Never Upload, Store, or Host Image Files
* **Specification:** Campus Buddy stores photos only as text URL strings pointing to public images. No image file is accepted, stored, or served by the application.
* **Done when:** The application has no file upload input, and the database contains no image binary data.