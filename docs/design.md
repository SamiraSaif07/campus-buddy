# Design

Five screens, mobile-first. Every field named below exists in `data-model.md`. Example data uses realistic ASU students. Layout, states and wording here are the reference for every screen ticket in `tickets/`.

## Visual system

**Layout**
* Designed for a 390–420px phone width, one column.
* On a laptop or projector, the app sits in a centered phone-width column on the cream page background; it never stretches into wide multi-column layouts.

**Colour** (4 colours plus charcoal text)

| Role | Colour | Used for |
| :--- | :--- | :--- |
| Page background | Warm Cream `#F7F3EE` | Every screen's background |
| Surfaces | White `#FFFFFF` | Cards, inputs, list rows, modal sheet |
| Primary accent | Warm Coral `#F26B3A` | Like, Save Profile, Open WhatsApp, active states, Match Revealed backdrop |
| Secondary accent | Sage Mint `#BCE0D3` (pills) and `#4BA38A` (icons) | Overlap Score badge, landmark tag, verified tick, "Study" pill |
| Text | Charcoal `#1E1E1E` | All body text and headings |
| Placeholder | Warm neutral `#EFE9E0` | Empty extra-photo slots on the card back, loading placeholders |

**Contrast rules** (checked against WCAG AA)
* Charcoal on cream (15:1), white (17:1) or sage pills (11.7:1): fine for all text.
* White text on coral is only 3:1. Use it only for **bold labels of 19px or larger** (buttons, the "It's a match!" heading). Smaller text on coral uses charcoal (5.5:1).
* Never use coral or `#4BA38A` for body text on cream or white (under 3:1). They are for buttons, icons and accents only.

**Personality**
Friendly, cozy and campus-oriented. Empty states and headers leave room for small hand-drawn doodles (to be drawn later; not required to ship). Colour inspiration came from Tantan; only the palette mood is borrowed, never its logo, mascot or screens.

**Motion**
* The card flip (R14) is a 3D turn of about 0.4s.
* Students with reduced motion enabled get an instant swap instead.
* Every gesture also has a visible button.

## Navigation

* **Bottom tab bar** on Screens 2, 3 and 5: **Me** (Screen 2) · **Deck** (Screen 3) · **Buddies** (Screen 5, with a count badge). The active tab has a coral icon, a bold charcoal label and a short coral bar above it. Screens 1 and 4 have no tab bar, and neither does Screen 2 while a new student fills it in for the first time (there is no deck or buddy list to visit yet).
* No session in LocalStorage → Screen 1, welcome view.
* Screen 1 welcome, **Sign up with university email** → sign-up view → code view → Verified → Screen 2.
* Screen 1 welcome, **Log in** → log-in view → correct email and password → Screen 3. Returning students never see the code view.
* Sign-up with an email that already has an account → error with a link to the log-in view.
* Saved session the server accepts → Screen 3; a session it rejects → Screen 1.
* Screen 2, Save Profile succeeds → Screen 3.
* Screen 3, flip button → card back and back again.
* Screen 3, a right-swipe that completes a mutual match → Screen 4 modal over Screen 3; "Continue matching" closes it and shows the next card.
* Screen 5, tap a buddy → Screen 4 modal over Screen 5; "Back to Buddies" closes it.
* The Me, Deck and Buddies tabs work in every Screen 3 state, including Daily Cap Reached and Deck Exhausted.

## Screen 1 — Welcome, Sign Up & Log In

**Serves:** R1, R2, R12, N9

**Layout, five views**
1. **Welcome view:** the Campus Buddy logo (two overlapping circles, coral and sage, with small smiles), the name "Campus Buddy", the line "Find a study or hangout buddy at ASU.", the pledge in a sage pill ("Strictly for platonic study buddies and campus friends at ASU."), a coral **Sign up with university email** button, a white **Log in** button with a charcoal outline, and the small line "ASU students only · @students.asu.edu.jo".
2. **Sign-up view:** back arrow; heading "Sign up with your university email"; the line "We use it to check you're an ASU student. Nobody else sees it."; **University email** (`email`), placeholder `202XXXXXX@students.asu.edu.jo`; **Create a Campus Buddy password** with a show/hide eye button and the hint "Your Campus Buddy password, not your ASU portal password."; the pledge pill; the error banner slot; a coral **Continue** button.
3. **Code view:** back arrow; lock icon in a sage circle; heading "Enter your 6-digit campus code"; "Signing up as `<email>`" with a **Change email** link; six digit boxes (one real input underneath, so pasting and screen readers work); the note "Demo version: use the code from the Campus Buddy team." in a dashed box; the error banner slot; a coral **Verify** button; "No code yet? **Resend code**". The copy never says a code was sent to the student's inbox, because no email is sent (Risk 7).
4. **Verified view:** sage tick, "Verified", "You're in, ASUer. Let's set up your profile." and a coral **Set up my profile** button to Screen 2. The app does not know the student's name yet; ASU emails are student numbers.
5. **Log-in view:** back arrow; heading "Welcome back, ASUer"; the line "Log in with your university email and your Campus Buddy password."; **University email**; **Campus Buddy password** with the eye button; the error banner slot; a coral **Log in** button; "New here? **Sign up**". A correct email and password go straight to Screen 3, with no code step.

No Student row is created on this screen. After sign-up, the email and the password are carried to Screen 2 in frontend state and stored (the password only as `password_hash`) on the first Save Profile.

**Empty state**
* Sign-up view: both fields blank, Continue disabled until both have text.
* Code view: six empty boxes, the first outlined in charcoal, Verify disabled, Resend code available.
* Log-in view: both fields blank, Log in disabled until both have text.

**Loading state**
* Sign-up: Continue shows a spinner and "Checking…" while the server checks the email is free; both fields are locked.
* Code: Verify shows a spinner and "Verifying…"; the boxes are locked; Resend is hidden.
* Log in: Log in shows a spinner and "Logging in…"; both fields are locked.

**Error state**
* Email not in the student format (R1): banner "Use your ASU student email, e.g. 202400000@students.asu.edu.jo." The field keeps the text and is outlined.
* Email already registered (R12): banner "This email already has an account." with a **Log in** link.
* Password under 8 characters: the password field is outlined with "At least 8 characters" under it.
* Wrong code (R2): banner "That code is not correct. Try again." The boxes clear and are outlined; Resend code stays available.
* Resend code (demo only): a sage note "Demo version: the code does not change. Ask the Campus Buddy team for it."
* Wrong log-in (R12): banner "Incorrect email or password." for both an unknown email and a wrong password, so the screen never reveals who has an account. The email is kept; the password clears.
* Network failure: banner "Can't reach Campus Buddy right now. Check your connection and try again." with **Retry**; input is kept.

**Full state**
* Sign-up: `202410587@students.asu.edu.jo` and a 10-character password, Continue enabled.
* Code: six digits typed, Verify enabled; on success the Verified view shows, then Screen 2.
* Log in: the demo account `202400000@students.asu.edu.jo` with its password goes straight to Screen 3.

## Screen 2 — The 'Me' Profile Suite & Schedule Panel

**Serves:** R3, R4, R5, R6, R15, N5, N6, N8

**Layout, top to bottom** (one scrolling form on white cards; required fields marked *)
1. **Photos**
   * Main photo URL* (`main_photo_url`) with a square preview once it passes.
   * Extra photos (`mood_photo_urls`): three URL slots labelled "Back of your card". Slot 1 is required*, slots 2 and 3 optional. Each shows a small preview once it passes.
   * Photo error banner and the 3-strike countdown slot (R4) under the photo fields.
2. **About you**
   * First name* (`first_name`), Last name* (`last_name`) with "Only shown after a mutual match".
   * Major* (`major`): free text with offline suggestions.
   * Academic year* (`academic_year`).
   * Gender* (`gender`): Male / Female, with "Used only to match you with the right people. Never shown on your card."
   * Who you'd like to meet* (`gender_preference`): a two-option toggle. A female student sees **Only Girls** (selected by default) / **Both**; a male student sees **Only Boys** (default) / **Both**. It appears once Gender is chosen.
   * Buddy preference* (`buddy_preference`): Study / Hangout / Both.
3. **Prompts** (`prompts`): three slots. Each has a prompt picker (the 20 from `prompts.md`, already-used prompts greyed out) and an answer box with a live "0/150" counter. Slot 1 is required*.
4. **Optional**
   * MBTI (`mbti`): picker of the 16 types, blank by default.
   * Birthday (`birthday`): month and day only.
   * Interest tags (`interest_tags`): type and press Enter, up to 5 chips.
   * Preferred ASU landmark (`preferred_landmark`).
5. **Contact (revealed only after a mutual match)**
   * WhatsApp number* (`whatsapp_number`): "International format, e.g. 9627XXXXXXXX".
   * Instagram (`instagram_handle`) and Discord (`discord_handle`).
6. **Free blocks** (`schedule_blocks`): 5×5 grid, Sun–Thu × the five blocks. Free cells fill sage; the counter reads "6 free blocks selected".
7. **Save Profile**: coral button fixed to the bottom.

**Empty state** (new student)
All fields blank; the gender preference toggle hidden until Gender is chosen; prompt slots show "Pick a prompt"; extra photo slots show `#EFE9E0` placeholders; grid empty, counter "0 free blocks selected". Intro line: "Set up your profile. Fields marked * are required." Save disabled until all 11 required fields are filled.

**Loading state**
* Photo check: the URL field shows a spinner and "Checking photo link…"; Save is disabled while any check runs.
* Opening for edit: grey `#EFE9E0` bars in place of each field until data arrives.
* Saving: Save shows a spinner and "Saving…"; the form is locked.

**Error state**
* Photo check fails (R3): banner with the exact server reason, e.g. "Photo link must start with https://" or "Photo link must end in .jpg, .jpeg, .png or .webp", plus "2 attempts left".
* Third consecutive failure (R4): photo fields disabled; banner "Too many invalid photo links. Try again in 14:59", counting down, with "Use clear, appropriate photos of yourself."
* Missing required field, no extra photo, no prompt answer, or bad WhatsApp format (R5): each problem field outlined with one line ("Required", "Add at least one photo for the back of your card", "Answer at least one prompt", "Use digits only, e.g. 9627XXXXXXXX"); the page scrolls to the first.
* Prompt answer over 150 characters: the counter turns coral at 150 and typing stops.
* A preview image that no longer loads: grey box "Image can't be loaded"; the URL stays editable.
* Network failure on save: banner "Couldn't save your profile. Your changes are still here." with **Retry**.

**Full state**
Lina Haddad, Computer Science, Second Year, Female, Only Girls, Study. Main photo plus 2 extra photos. Prompts: "My go-to spot on campus is… because…" → "360 Square between lectures, because the shade and the falafel stand win every time." and "The class I'd happily explain to anyone is…" → "Structured Programming. I tutor it, so bring your loops!" MBTI INFJ, birthday 03-14, tags "Java OOP", "Competitive programming", "Coffee", "Chess", landmark 360 Square. WhatsApp 962791234567, Instagram `@lina.codes`. Grid with 6 free blocks. Save enabled.

## Screen 3 — The Main Swipe Deck

**Serves:** R7, R8, R9, R10, R14, R15, N3, N4, N5, N7

**Layout**
1. **Header and tab bar:** the logo and "Campus Buddy" at the top; the tab bar at the bottom with Deck active (see Navigation).
2. **Card front** (white, rounded):
   * Main photo filling the top two-thirds.
   * First name in large charcoal text, with the "🍰 B-DAY TODAY!!" tag when it applies (R10).
   * Overlap Score badge in a sage pill ("3 matching windows"), or the R8 notice in its place.
   * Major · Academic year on one line; Buddy preference as a pill.
   * Flip button (↻ "More about Lina") in the top-right corner of the card.
3. **Card back** (same card, turned over):
   * Left column: three photo slots stacked top, middle, bottom. Filled slots show the extra photos; empty slots are `#EFE9E0` boxes.
   * Beside and between the photos, zig-zagging down the card: each prompt question in small bold text with its answer underneath.
   * Bottom: interest tag chips, MBTI, and the landmark in a sage pill with a pin icon.
   * The back scrolls inside the card if the content is taller than the card; the flip button stays fixed in the corner.
4. **Pass (✕)** in white with a charcoal icon, and **Like (♥)** in coral, under the card. They work from either side; swiping the card does the same.

Never on any card: last name, WhatsApp, Instagram, Discord (N4), birthday date or age (N5), or gender (R9).

**Empty state**
* **Deck Exhausted** (R7): the card area shows a doodle space and "You've seen everyone for now. New students join all the time — check back later." The Buddies tab still works.
* **Daily Cap Reached** (N3): "That's your 15 for today. Come back tomorrow for a fresh deck." The Buddies tab still works.
* **Card back with 1 extra photo and 1 prompt:** two placeholder slots; the prompt sits beside the first photo; the rest of the back stays in its usual place.

**Loading state**
* Deck loading: a card-shaped `#EFE9E0` placeholder with pulsing bars; Pass, Like and flip disabled.
* After a swipe: the card animates off; the buttons stay disabled until the server confirms; then the next card appears front-side up.

**Error state**
* Deck fails to load: "Couldn't load your deck." with **Retry**.
* Swipe fails to save: the card slides back; banner "Your swipe didn't go through. Try again."
* Duplicate swipe rejected (N7): the card is skipped silently and the next one loads.
* Session rejected: route to Screen 1.
* A main photo that won't load: grey area with the first-name initial; the rest of the card renders. An extra photo that won't load on the back shows a `#EFE9E0` slot with a small broken-image line icon.

**Full state**
Every example below is what Lina (Only Girls) sees, so every card and buddy is a girl (R15).
* Front: Rania, 🍰 B-DAY TODAY!!, "3 matching windows", Computer Science · Second Year, Study pill. Buddies badge shows 2.
* Back: three extra photos down the left. "Outside of class, you'll find me…" → "At the gym or losing at FIFA to my roommate." and "A study habit I swear by is…" → "Pomodoro and a good playlist." Tags "Java OOP", "Gym", "Football"; INTP; landmark pill "360 Square".
* R8 example: Dana, Software Engineering, Third Year, Study, with the zero-overlap notice in place of the badge.

## Screen 4 — The 'Match Revealed' Modal

**Serves:** R11, N1, N4

**Layout** (coral backdrop over Screen 3 or Screen 5; white sheet in the middle)
1. "It's a match!" in bold white on the coral backdrop. When opened from Screen 5, the heading is the buddy's first name instead.
2. Main photo in a circle.
3. First and Last name, Major · Academic year.
4. Instagram and Discord rows, only if present.
5. **Open WhatsApp**: full-width coral button with a bold white label, opening `https://wa.me/<whatsapp_number>`.
6. **See their card**: shows the card back (R14) inside the sheet, with "Back" to return.
7. **Continue matching** (from Screen 3) or **Back to Buddies** (from Screen 5).

No message box, chat field or inbox (N1).

**Empty state**
No Instagram or Discord: those rows are hidden. Name, Major, Year and the WhatsApp button always appear (required fields).

**Loading state**
The backdrop, heading and photo circle appear immediately; name and contact rows show `#EFE9E0` bars; Open WhatsApp is disabled until the number arrives.

**Error state**
Details fail to load: "You matched! We couldn't load their details right now. You'll find them on your Buddies screen." with **Close**. The match is already stored, so it appears on Screen 5.

**Full state**
"It's a match!" Rania's photo. "Rania Odeh", Computer Science · Second Year. Instagram `@rania.o`, Discord `rania#2048`. Open WhatsApp active, See their card, Continue matching.

## Screen 5 — History Buddies

**Serves:** R13, R11, N4

**Layout**
1. Header: the logo and the title "Your buddies" with the count; the tab bar at the bottom with Buddies active.
2. Scrollable list on cream, one white row per buddy, newest match first:
   * Left: main photo thumbnail (circle), full name, and major under it.
   * Right: WhatsApp icon button (coral), Instagram and Discord icon buttons (charcoal outline), shown only if present.
   * Tapping anywhere outside the icon buttons opens Screen 4 for that buddy.

**Empty state**
Doodle space and "No buddies yet. When someone you liked likes you back, they'll appear here." with a coral **Back to the deck** button.

**Loading state**
Three `#EFE9E0` placeholder rows with a circle and two bars each.

**Error state**
* List fails to load: "Couldn't load your buddies." with **Retry**.
* A buddy's photo won't load: circle with their first-name initial.
* Session rejected: route to Screen 1.

**Full state**
* Rania Odeh, Computer Science: WhatsApp, Instagram, Discord.
* Yara Nasser, Pharmacy: WhatsApp, Instagram.
* Huda Saleh, Civil Engineering: WhatsApp only.

## Schema Updates Needed

None. The fields this design needs (`gender_preference`, `prompts`, `mood_photo_urls` with 1–3 entries, `gender` required) were added to `data-model.md` on 2026-11-19.
