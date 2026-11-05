# Project Brief: Campus Buddy (ASU Matcher)

## 1. The Problem

First-year and introverted undergraduates at Applied Science Private University (ASU) experience severe social anxiety, isolation, and institutional friction. The sheer scale of the campus makes finding compatible study or social peers intimidating.

Today, students try to bridge this gap through massive, unstructured 500+ member course WhatsApp groups. In these groups, peer-to-peer study plans and genuine attempts at social connection get lost instantly under a wall of chat noise. There is also no structured way for students to filter potential peers by compatible schedule windows, shared academic courses, or specific campus landmarks. (Comparing shared courses is a post-MVP feature; the MVP matches on schedule overlap, major, and buddy preference.)

**What it costs them:** hours of wasted effort, heightened social anxiety, safety concerns from exposed phone numbers, and an isolated university experience.

## 2. Who Has It

* **Primary:** First-year, introverted, and isolated undergraduate students at ASU who struggle to build platonic, high-value peer networks.
* **First user:** The project owner, a second-year Computer Science student at ASU actively seeking structured study groups and platonic campus connections.
* **Language:** English, suited to the tech-focused, structured academic faculty environment.
* **Evidence:** The project owner's own lived experience, plus recognized isolation patterns among new and isolated students on the ASU campus.

## 3. Today, Without It

A typical search for a study partner or campus friend looks like this:

1. Join an overpopulated, unmoderated 500+ member course WhatsApp group.
2. Post a manual request for a study partner or hangout buddy.
3. Watch the message get buried instantly under hundreds of unrelated messages, memes, and logistics questions.
4. Try to vet individuals manually by tapping raw phone numbers in the group chat, which creates social friction and safety concerns.
5. Spend hours manually checking whether a potential peer has overlapping free lecture blocks or uses the same parts of campus.
6. **Result:** Communication fatigue, heightened social anxiety, reliance on superficial connections, and an isolated university experience.

## 4. What Success Looks Like

On Demo Night, a user can:

* Enter through a single passwordless Registration/Sign-In gateway on Screen 1: type an email that passes the exact ASU student format check (`@students.asu.edu.jo`), then complete simulated registration string code verification by entering a 6-digit mock code.
* See the app switch between Pending vs Verified State UI views, then be routed by account status: a new email continues to profile setup (Screen 2), while a recognized email goes straight to the swipe deck (Screen 3) with the existing profile intact and no duplicate account.
* Return later on the same browser without signing in again, because the session is kept in browser LocalStorage.
* Build a profile with a main photo and optional mood photos supplied as text URL strings pointing to public images, alongside major, academic year, buddy preference, interest tags, preferred ASU landmark, optional Instagram and Discord handles, and weekly free blocks.
* View a daily deck of student profile cards ordered by schedule overlap, each showing first name, photos, major, academic year, buddy preference (Study, Hangout, or Both), up to 5 interest tags, overlap score, and preferred ASU landmark.
* Swipe right (like) or left (pass), and on a mutual right-swipe, instantly see the other student's full name, major, academic year, Instagram and Discord handles, and a WhatsApp link on a dedicated "Match Revealed" screen.
* Reopen any earlier match at any time from the toggleable "Matches Drawer" on the swipe deck, which lists each match's first name with a WhatsApp action button.

**Demo data:** The database will be pre-seeded with 20 mock profile records covering diverse ASU majors, interest tags, schedule blocks, landmarks, and public photo URLs. This bypasses the cold-start problem so the swipe deck, overlap scores, and match flow are fully demonstrable during the live Demo Night presentation.

### Measures

| Measure | Baseline (today) | Target (week 8) |
| :--- | :--- | :--- |
| Time to discover a compatible peer | Hours spent filtering crowded WhatsApp chats | Minutes within a single, intentional swipe session |
| Pre-meeting communication friction | High anxiety over open-ended small talk | Immediate hand-off to direct WhatsApp with shared context (major, free blocks, landmark) |
| Daily profile card consumption | Infinite, unstructured scrolling | Capped at exactly 15 deliberate cards per user per day |
| Closed campus network | Unmoderated public phone numbers | Every account passes the `@students.asu.edu.jo` format check and simulated registration string code verification |
| Match retention | Contacts lost in group chat scroll | Every mutual match stays reachable from the Matches Drawer |
| Demo readiness | No data to match against | 20 seeded profiles available from the first login on Demo Night |

## 5. Scope

### In scope (8 weeks)

* A localized, privacy-first, platonic matchmaking responsive web application tailored exclusively to ASU, designed mobile-first.
* **Unified passwordless Registration/Sign-In gateway (Screen 1):**
  * The email must exactly match the `@students.asu.edu.jo` format.
  * Simulated registration string code verification: students type a 6-digit mock code as a text string; no document or file is submitted.
  * Verification status is held in local frontend state, rendered as Pending vs Verified State UI views.
  * After verification, the backend looks up the email: a new email routes to profile setup (Screen 2); a recognized email routes directly to the swipe deck (Screen 3) without creating a duplicate account.
  * Sessions persist in browser LocalStorage, so a returning user on the same browser bypasses Screen 1.
* **Profile photos as text URL strings:**
  * Each profile stores one main photo and up to 4 optional mood photos as text URL strings pointing to publicly hosted images.
  * The app stores and renders the URL only. No image file is uploaded, stored, or hosted by Campus Buddy, so image hosting costs are zero.
  * Every submitted or edited photo URL passes a simulated Vision validation check before the profile can be saved, with a limit of 3 consecutive failed submissions per session.
* **Profile data:** required Full Name, Major, Academic Year, Buddy Preference, WhatsApp number, and main photo URL; optional Gender, MBTI, Birthdate, up to 5 interest tags, preferred ASU landmark, mood photo URLs, and social handles.
* **Social handles:** limited strictly to Instagram and Discord handles. No other social platforms are collected or displayed.
* **Profile editing:** users can return from the swipe deck to Screen 2 to edit their saved profile.
* **Daily discovery cap:** exactly 15 student profile cards per user per calendar day, counted from a log of viewed cards, to protect database health and give users a reason to return.
* **Block-based availability scheduling:** a 5×5 grid of standard blocks (8:30–10:00, 10:00–11:30, 11:30–1:00, 1:00–2:30, 2:30–4:00) across Sunday to Thursday. Users toggle the blocks they are free, and the deck is ordered by Overlap Score (e.g., "3 matching free windows").
* **ASU landmark selection:** a predefined list of campus landmarks (e.g., IT Faculty Lab, 360 Square, Main Library) shown on the card so mutual matches can decide where they'd prefer to meet.
* **Birthday tag:** a "B-DAY TODAY!!" tag on cards whose birthdate matches the current day, without showing the date or age.
* **Zero-chat match gateway:** contact details stay locked until a mutual right-swipe, then surface immediately on the "Match Revealed" screen (Screen 4): full name, major, academic year, Instagram and Discord handles, and a WhatsApp link.
* **Matches Drawer (Screen 3):** a toggleable drawer on the swipe deck that stores and lists every mutual match with first name and a WhatsApp action button, available even after the daily cap is reached.
* **Seed data:** 20 pre-seeded mock profiles for development and Demo Night.

### Out of scope

* Passwords, password resets, and any other sign-in method beyond the passwordless gateway.
* Any admin dashboard, manual review queue, or file verification workflow.
* Image file uploads, image storage, or image hosting of any kind. Photos exist only as text URL strings.
* Social platforms other than Instagram and Discord.
* Live automated SMTP email verification servers.
* AI-driven OCR document scanning for student verification (a possible post-MVP direction).
* Checking registration codes against real university records.
* Filtering the card deck by landmark or buddy preference.
* In-app text chat sockets, real-time message synchronization, message inboxes, or thread management.
* Unlimited profile swiping.
* Multi-campus deployment or cross-university database indexing.
* Native iOS or Android mobile application builds.

### Constraints

* **One developer:** a solo creator owning product architecture and codebase delivery.
* **Timeline:** a fixed 8-week delivery window under the IEEE WIE JS Mentorship Program, with one mentor sync per week and 3–8 hours of independent development between sessions.
* **One AI tool:** a single Claude Pro subscription for guidance and tutoring.
* **Zero financial cost:** free-tier hosting and infrastructure only; no paid image storage or paid runtime APIs.

## 6. Risks and Unknowns

| # | Risk or unknown | Why it matters | How it will be resolved |
| :- | :--- | :--- | :--- |
| 1 | Scope creep on the verification flow | Building automated email systems, document scanners, or an admin review dashboard would break the 8-week timeline. | Simulated registration string code verification with the exact email format check, a 6-digit mock code, and Pending vs Verified State UI views in local frontend state. Real verification is deferred until after the MVP. |
| 2 | Database strain from heavy usage | Uncapped swiping can exhaust free-tier database reads and writes during peak campus hours. | Enforce the 15-card daily cap at the query layer by counting the user's viewed-card records for the current calendar date. |
| 3 | Empty deck on Demo Night (cold start) | With no real users, the swipe deck, overlap scores, and match flow would have nothing to show. | Pre-seed 20 mock profiles with diverse ASU majors, interest tags, schedule blocks, landmarks, and public photo URLs. |
| 4 | User hesitation to move to external apps | Introverted users may feel friction when leaving the platform to message someone on WhatsApp. | Use the profile structure to state explicit intent up front (e.g., "Java OOP study group at the IT lab," "morning coffee chat with someone from the AI majors"), giving students a clear reason to text. |
| 5 | Externally hosted photo URLs | Campus Buddy does not control the image behind a URL: it can break, change, or be replaced with inappropriate content after the profile is saved. | Validate each URL with the simulated Vision check on every submit or edit; accept that later changes to the external image are not re-checked in the MVP. |
| 6 | Data architecture scalability | Hard-coding campus parameters too early could make expanding to other universities difficult later. | Build ASU landmarks and schedule blocks as modular arrays in the data schema so they can accept new university inputs post-MVP. |
| 7 | Mock Security Gate Vulnerability in MVP | Because a single mock 6-digit verification code string is used to bypass SMTP server building overhead, any user could theoretically input a peer's ASU email to view their matches' WhatsApp link details. | Accepted as a standard, calculated risk for a simulated demo day prototype. Production versions post-MVP will implement true independent SMTP closed-loop verification servers. |