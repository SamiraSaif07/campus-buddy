# Design

Four screens, mobile-first, one column. Every field named below exists in `data-model.md` unless it is listed under **Schema Updates Needed** at the bottom. Example data uses realistic ASU students.

## Navigation

* No session in Local Storage → Screen 1.
* Screen 1, new email, correct code → Screen 2.
* Screen 1, recognized email, correct code → Screen 3.
* Saved session in LocalStorage that the server accepts → Screen 3.
* Saved session the server rejects → Screen 1.
* Screen 2, "Save Profile" succeeds → Screen 3.
* Screen 3, edit button → Screen 2 (pre-filled).
* Screen 3, right-swipe that completes a mutual match → Screen 4 opens as a modal over Screen 3.
* Screen 4, close → back to Screen 3, next card.

## Screen 1 — Registration & Sign-In Gateway

**Serves:** R1, R2, R12

**Layout, top to bottom**
1. App name "Campus Buddy" and one line: "Find a study or hangout buddy at ASU."
2. **Email field** (`email`). Placeholder: `name@students.asu.edu.jo`.
3. **Continue** button.
4. After the email passes R1, the same card switches to the **Pending view**: the email shown read-only with a "Change" link, a **6-digit code field** (numeric, 6 characters), and a **Verify** button.
5. **Error banner** slot directly above the active button.

Verification status lives in frontend state only (R2). No Student row is created on this screen; the email is carried to Screen 2 in frontend state (see Schema Updates Needed, item 3).

**Empty state**
Email field blank, Continue button disabled until the field has text. No banner. The code field is not shown yet.

**Loading state**
After Continue: the button shows a small spinner and the label "Checking…", and the field is locked. After Verify: same treatment, label "Verifying…". Nothing else on the card moves.

**Error state**
* Wrong suffix (R1): red banner "Use your ASU student email ending in @students.asu.edu.jo." The field keeps the typed text and is highlighted.
* Wrong code (R2): red banner "That code is not correct. Try again." Code field clears; the user stays in the Pending view.
* Network failure: banner "Can't reach Campus Buddy right now. Check your connection and try again." with a **Retry** button. Typed input is kept.

**Full state**
Pending view showing `lina.haddad@students.asu.edu.jo`, code field filled with six digits, Verify enabled. On success, the card briefly shows the Verified view ("Verified ✓") before routing: Lina is new, so she goes to Screen 2; a returning student such as `omar.khalil@students.asu.edu.jo` goes straight to Screen 3.

## Screen 2 — The 'Me' Profile Suite & Schedule Panel

**Serves:** R3, R4, R5, R6, N5, N6

**Layout, top to bottom** (one scrolling form; required fields marked with an asterisk)
1. **Photos**
   * Main photo URL* (`main_photo_url`) with a small square preview once it passes the check.
   * Up to 4 mood photo URL fields (`mood_photo_urls`), added one at a time with "+ Add mood photo".
   * Photo error banner and 3-strike countdown slot (R4) directly under the photo fields.
2. **About you**
   * First name* (`first_name`), Last name* (`last_name`), with the note "Only shown after a mutual match" under Last name.
   * Major* (`major`): free text with suggestions from the Major Suggestions list.
   * Academic year* (`academic_year`): select from the Academic Years list.
   * Buddy preference* (`buddy_preference`): three-way toggle Study / Hangout / Both.
3. **Optional**
   * Gender (`gender`): select from Gender Options, with a blank "Prefer not to say" default.
   * MBTI (`mbti`): select of the 16 four-letter types, blank default.
   * Birthday (`birthday`): month and day pickers only, no year (N5).
   * Interest tags (`interest_tags`): type and press Enter to add a chip, up to 5; each chip has a remove ×.
   * Preferred ASU landmark (`preferred_landmark`): select from ASU Landmarks.
4. **Contact (revealed only after a mutual match)**
   * WhatsApp number* (`whatsapp_number`): digits only, helper text "International format, e.g. 9627XXXXXXXX".
   * Instagram handle (`instagram_handle`), Discord handle (`discord_handle`).
5. **Free blocks** (`schedule_blocks`): 5×5 grid. Columns Sun–Thu, rows 8:30–10:00, 10:00–11:30, 11:30–1:00, 1:00–2:30, 2:30–4:00. Tapping a cell toggles it filled (free) or empty (busy). A counter under the grid reads "6 free blocks selected".
6. **Save Profile** button, fixed to the bottom of the viewport.

**Empty state** (new student, first visit)
All fields blank, toggles unselected, grid all empty, counter "0 free blocks selected". Short intro at the top: "Set up your profile. Fields marked * are required." Save Profile is visible but disabled until all seven required fields are filled.

**Loading state**
* Photo check: the URL field shows a spinner and "Checking photo link…"; Save is disabled while any check runs.
* Opening the screen to edit an existing profile: grey placeholder bars in place of each field for the moment before data arrives, then the form fills in.
* Saving: Save Profile shows a spinner and "Saving…"; the form is locked.

**Error state**
* Photo check fails (R3): red banner with the exact reason the server returned, e.g. "Photo link must start with https://" or "Photo link must end in .jpg, .jpeg, .png or .webp". Attempts left are shown: "2 attempts left".
* Third consecutive failure (R4): photo fields disabled, banner "Too many invalid photo links. Try again in 14:59", counting down live, plus a short campus compliance note: "Use a clear, appropriate photo of yourself."
* Missing required field or bad WhatsApp format (R5): save is rejected; each problem field is outlined red with one line under it ("Required", "Use digits only, e.g. 9627XXXXXXXX"), and the page scrolls to the first one.
* A previously valid image no longer loads in its preview: the preview shows a grey box with "Image can't be loaded" and the URL stays editable.
* Network failure on save: banner "Couldn't save your profile. Your changes are still here." with **Retry**.

**Full state**
Main photo preview loaded; mood photos 2 of 4. First name Lina, Last name Haddad, Major "Computer Science", Academic year "Second Year", Buddy preference **Study**. Gender Female, MBTI INFJ, Birthday 03-14. Tags: "Java OOP", "Competitive programming", "Coffee", "Chess". Landmark "360 Square". WhatsApp 962791234567, Instagram `@lina.codes`. Grid: Sun 10:00–11:30, Mon 11:30–1:00, Tue 10:00–11:30, Wed 1:00–2:30, Thu 8:30–10:00 and 10:00–11:30 filled; counter "6 free blocks selected". Save Profile enabled.

## Screen 3 — The Main Swipe Deck

**Serves:** R7, R8, R9, R10, R13, N3, N4, N5, N7

**Layout, top to bottom**
1. **Top bar:** "Campus Buddy" on the left; on the right a **Matches** button with a count badge (opens the Matches Drawer) and an **Edit profile** button (to Screen 2).
2. **Card** (one at a time, the top of the deck):
   * Main photo filling the top half; mood photos as small thumbnails or swipeable in the photo area.
   * First name (`first_name`), with the "🍰 B-DAY TODAY!!" tag beside it only when `birthday` matches today in Asia/Amman (R10).
   * Overlap Score badge, e.g. "3 matching windows" (computed, R7).
   * Zero-overlap notice when R8 applies: "0 Overlapping Windows — Schedules conflict, but you share a major or buddy preference!"
   * Major, Academic year, Buddy preference, Gender, MBTI, interest tag chips, preferred landmark with a pin icon.
   * Optional fields that are empty are not shown at all (no blank rows).
   * Never on the card: last name, WhatsApp, Instagram, Discord (N4), or any birthday date or age (N5).
3. **Pass (✕)** and **Like (♥)** buttons under the card. Swiping the card left or right does the same.
4. **Matches Drawer:** slides up from the bottom; one row per mutual match with first name and a **WhatsApp** button (R13). Close with a handle or tap outside.

**Empty state**
* **Deck Exhausted** (no unswiped candidates remain, R7): card area replaced by a message: "You've seen everyone for now. New students join all the time — check back later." Matches button still works.
* **Daily Cap Reached** (15 swipes today, N3): card area replaced by "That's your 15 for today. Come back tomorrow for a fresh deck." Matches button still works.
* **Matches Drawer with no matches:** "No matches yet. When someone you liked likes you back, they'll appear here."

**Loading state**
* Deck loading: a grey card-shaped placeholder with pulsing bars where the name, badge and chips will be; Pass and Like are disabled.
* After a swipe: the card animates off, the buttons stay disabled until the server confirms the swipe, then the next card appears.
* Drawer loading: three grey placeholder rows.

**Error state**
* Deck fails to load: card area shows "Couldn't load your deck." with **Retry**.
* A swipe fails to save: the card slides back into place with a banner "Your swipe didn't go through. Try again."
* Duplicate swipe rejected (N7): the card is removed from view silently and the next card loads; no error is shown, because the server already has that swipe.
* Session rejected by the server: route to Screen 1.
* A card's photo URL no longer loads: grey photo area with the first name's initial in the centre; the rest of the card displays normally.
* Drawer fails to load: "Couldn't load your matches." with **Retry**.

**Full state**
Card for Omar: main photo, "Omar" with the 🍰 B-DAY TODAY!! tag, badge "3 matching windows", Computer Science, Second Year, Buddy preference Study, Male, INTP, tags "Java OOP", "Gym", "Football", landmark "360 Square". Matches badge shows 2. Drawer lists "Yara — WhatsApp" and "Ahmad — WhatsApp".
Second example, R8: card for Dana, Software Engineering, Third Year, Buddy preference Study, badge replaced by the zero-overlap notice.

## Screen 4 — The 'Match Revealed' Screen Gateway

**Serves:** R11, N1

**Layout** (modal over Screen 3, dimmed background)
1. Heading "It's a match!"
2. Matched student's main photo.
3. First name and Last name (`first_name`, `last_name`), Major, Academic year.
4. Instagram handle and Discord handle, each shown only if present.
5. **Open WhatsApp** button, full width, opening a click-to-chat link built from `whatsapp_number`.
6. **Keep swiping** link that closes the modal.

No message box, chat field or inbox on this screen (N1).

**Empty state**
The match has no Instagram or Discord handle: those rows are not shown. Name, Major, Academic year and the WhatsApp button always appear, because they are required fields.

**Loading state**
The modal opens immediately with the heading and photo area; name and contact rows show grey placeholder bars until the match details arrive. The WhatsApp button is disabled until the number is loaded.

**Error state**
Match details fail to load: modal shows "You matched! We couldn't load their details right now. You'll find them in your Matches." with **Close**. The match is already stored, so it appears in the Matches Drawer (R13).

**Full state**
"It's a match!" Photo of Omar. "Omar Khalil", Computer Science, Second Year. Instagram `@omar.k`, Discord `omark#2048`. Open WhatsApp button active. Keep swiping link below.

## Schema Updates Needed

No new columns are required by these four screens. Three items are not yet written down anywhere and need a decision before tickets are cut:

1. **Mock verification code value.** R2 uses one shared 6-digit code, but no file states where it lives. Proposed: a server-side configuration value in `.env`, not a database field.
2. **Demo account identity.** `data-model.md` refers to "the demo account's `id`" for the seeded likes, but does not say whether the demo account is one of the 20 seeded Students or a 21st row, or which email it uses.
3. **When the Student row is created.** Screen 1 creates nothing, and every required field must exist at save time (R5). Proposed rule for `data-model.md`: the Student row and its `id` are created when Screen 2 is first saved, using the email carried from Screen 1. Until then, the session in LocalStorage is empty.