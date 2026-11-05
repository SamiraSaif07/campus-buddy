# Screens

All interfaces are designed as a responsive web application layout optimized mobile-first for clean presentation viewports.

### 1. Screen 1: Registration & Sign-In Gateway
* Purpose: Unified landing portal to onboard new users and authenticate returning users.
* UI Elements: Email text entry field, mock 6-digit verification code text box, error banner, and Pending vs Verified state views toggled by local state. After a verified code, a new email routes to Screen 2 and a recognized email routes directly to Screen 3. Returning sessions are saved via browser LocalStorage tokens to bypass this gateway entirely.
* Requirements Served: R1, R2, R12

### 2. Screen 2: The 'Me' Profile Suite & Schedule Panel
* Purpose: Account setup and data configuration workspace.
* UI Elements: Main photo URL text input, up to 4 optional mood photo URL fields, optional Instagram and Discord handle inputs, a 3-strike URL validation failure alert banner with a 15-minute countdown timer, Full Name text box, Major dropdown, Academic Year select list, Buddy Preference toggle (Study/Hangout/Both), WhatsApp number input field, optional selectors (Gender, MBTI, Birthdate), up to 5 interest tags, a preferred ASU landmark dropdown selection, and the 5x5 interactive schedule checkbox grid (Sunday-Thursday, 5 standard lecture blocks). Features a persistent "Save Profile" action button. Editing a saved photo URL re-runs the validation check before saving.
* Requirements Served: R3, R4, R5, R6, N6

### 3. Screen 3: The Main Swipe Deck
* Purpose: Core profile exploration loop showing potential campus matches.
* UI Elements: Viewport rendering 1 candidate card at a time. The card displays: First Name, photo URLs, Major, Academic Year, Gender, Buddy Preference, interest tags, MBTI, preferred ASU landmark, an Overlap Score Badge (e.g., "3 matching windows"), and a conditional "🍰 B-DAY TODAY!!" banner. Controls feature Left (pass) and Right (like) swipe buttons. A persistent navigation button allows users to return to Screen 2 to edit profile settings. A toggleable "Matches Drawer" lists the First Name and a direct WhatsApp action button for every active mutual match, and stays available when the cap is reached. Renders a blocking "Daily Cap Reached" screen when cap criteria are satisfied.
* Requirements Served: R7, R8, R9, R10, R13, N3, N4, N5

### 4. Screen 4: The 'Match Revealed' Screen Gateway
* Purpose: Post-match secure communication portal rendered only upon a mutual right-swipe.
* UI Elements: Confirmation modal listing the match's Full Name, Major, Academic Year, Instagram handle, and Discord handle, with a direct click-to-open WhatsApp action button.
* Requirements Served: R11, N1

---

## Data-Only Specifications
* Pre-seeded Profiles (N2): Executed as a backend database seed only. Contains no student-facing user interface or screens.

## Not Building Line
The application completely excludes: multi-user real-time socket chat layouts, message inboxes, admin review queues, global user search engines, filtering the card deck by landmark/buddy preference criteria, and university-wide data analytics tracking metrics.