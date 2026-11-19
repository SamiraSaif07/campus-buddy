# Screens

Five screens. Mobile-first: one column, designed for a 390–420px phone width. On a laptop the app sits in a centered phone-width column. Full layouts and the four states of each screen are in `design.md`.

### 1. Screen 1: Welcome, Sign Up & Log In
* Purpose: one entry point for new and returning students.
* UI Elements: welcome view (logo, pledge "Strictly for platonic study buddies and campus friends at ASU.", "Sign up with university email", "Log in"); sign-up view (student email, Campus Buddy password); code view (six digit boxes, "Demo version: use the code from the Campus Buddy team.", Resend code); Verified view ("You're in, ASUer."); log-in view (student email, Campus Buddy password). Sign-up routes to Screen 2; log-in routes to Screen 3 with no code step. A saved LocalStorage session skips this screen.
* Requirements Served: R1, R2, R12, N9

### 2. Screen 2: The 'Me' Profile Suite & Schedule Panel
* Purpose: create the profile on first visit; edit it later.
* UI Elements: bottom tab bar when editing (hidden during first-time setup); main photo URL; 3 extra photo URL slots (1 required); 3-strike photo check banner with 15-minute countdown; First and Last Name; Major free-text field with offline suggestions; Academic Year select; Gender select; Gender Preference toggle (own gender only, the default, or Both); Buddy Preference toggle (Study/Hangout/Both); WhatsApp number (international format); 3 prompt slots, each a prompt picker from `prompts.md` plus a 150-character answer box (1 required); optional MBTI, Birthday (month and day), up to 5 interest tags, preferred ASU landmark, Instagram and Discord; the 5×5 free-block grid (Sun–Thu) with a counter; a fixed "Save Profile" button.
* Requirements Served: R3, R4, R5, R6, R15, N5, N6, N8

### 3. Screen 3: The Main Swipe Deck
* Purpose: discover buddies one card at a time.
* UI Elements: bottom tab bar (Me · Deck · Buddies with a count badge, shared with Screens 2 and 5). One card at a time.
  * Front: main photo, first name with the "🍰 B-DAY TODAY!!" tag when it applies, Overlap Score badge (or the R8 notice), Major, Academic Year, Buddy Preference.
  * Back (in-place flip): 3 extra photo slots down the left (empty slots as warm-neutral placeholders), 1–3 prompts and answers beside them, interest tags, MBTI, preferred landmark.
  * Controls: Pass (✕), Like (♥), and a flip button.
* Blocking states:
  * "Daily Cap Reached": shown after 15 swipes on the current Asia/Amman date.
  * "Deck Exhausted": shown when no unswiped candidates remain.
  * The tab bar works in both.
* Requirements Served: R7, R8, R9, R10, R14, R15, N3, N4, N5, N7

### 4. Screen 4: The 'Match Revealed' Modal
* Purpose: celebrate a mutual match and hand off to WhatsApp.
* UI Elements: modal over Screen 3 (or Screen 5) with "It's a match!" heading, main photo, First and Last Name, Major, Academic Year, Instagram and Discord (if present), "Open WhatsApp" button, "See their card" control (shows the card back), and "Continue matching" (or "Back to Buddies" when opened from Screen 5).
* Requirements Served: R11, N1, N4

### 5. Screen 5: History Buddies
* Purpose: every match in one place, so a contact is never lost.
* UI Elements: the title "Your buddies" and the bottom tab bar; a scrollable list, newest first. Each row: main photo thumbnail, full name and major on the left; WhatsApp, Instagram and Discord buttons on the right. Tapping a row reopens Screen 4 for that buddy.
* Requirements Served: R13, R11, N4

---

## Data-Only Specifications
* Pre-seeded Profiles and Likes (N2): a backend database seed of 20 profiles, including the demo account (`202400000@students.asu.edu.jo`) and profiles that have already liked it. No student-facing screen.

## Not Building Line
The application excludes: in-app chat or message inboxes, a free-text "About me" bio, Sharespot-style music/book/movie folders (v2), community reporting or flagging (v2), admin review queues, user search, deck filters by landmark or buddy preference, an opposite-gender-only filter, account deletion, and analytics.
