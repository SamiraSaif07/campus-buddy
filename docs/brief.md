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

* See the pledge "Strictly for platonic study buddies and campus friends at ASU." on Screen 1, then sign up with an ASU student email (the student number before `@students.asu.edu.jo`) and a Campus Buddy password, and complete simulated code verification by entering a 6-digit mock code.
* See the Verified view ("You're in, ASUer.") and continue to profile setup (Screen 2); later, log in with the same email and Campus Buddy password and go straight to the swipe deck (Screen 3) with no code step and no duplicate account.
* Return later on the same browser without signing in again, because the session is kept in browser LocalStorage.
* Build a profile with a main photo and 1–3 extra photos supplied as text URL strings pointing to public images, 1–3 answers to curated campus prompts, major, academic year, gender (used only for matching), who they'd like to meet (same gender by default, or both), buddy preference, interest tags, preferred ASU landmark, optional Instagram and Discord handles, and weekly free blocks.
* View a daily deck of unswiped student profile cards ordered by schedule overlap. The front shows the main photo, first name, overlap score, major, academic year and buddy preference (Study, Hangout, or Both); flipping the card in place shows the extra photos, prompt answers, interest tags, MBTI and preferred ASU landmark.
* Swipe right (like) or left (pass), and on a mutual right-swipe, instantly see the other student's full name, major, academic year, Instagram and Discord handles, and a WhatsApp link in a "Match Revealed" modal over the deck.
* Reopen any earlier match at any time from the History Buddies screen, which lists every match with their WhatsApp and social buttons.

**Demo data:** The database will be pre-seeded with 20 mock profile records covering diverse ASU majors, interest tags, schedule blocks, landmarks, and public photo URLs, plus likes from selected mock profiles to the demo account (`202400000@students.asu.edu.jo`), which is one of the 20. This bypasses the cold-start problem so the swipe deck, overlap scores, and match flow are fully demonstrable during the live Demo Night presentation.

### Measures

| Measure | Baseline (today) | Target (week 8) |
| :--- | :--- | :--- |
| Time to discover a compatible peer | Hours spent filtering crowded WhatsApp chats | Minutes within a single, intentional swipe session |
| Pre-meeting communication friction | High anxiety over open-ended small talk | Immediate hand-off to direct WhatsApp with shared context (major, free blocks, landmark) |
| Daily profile card consumption | Infinite, unstructured scrolling | Capped at exactly 15 deliberate cards per user per day |
| Closed campus network | Unmoderated public phone numbers | Every account passes the digits-only `@students.asu.edu.jo` format check and simulated code verification |
| Match retention | Contacts lost in group chat scroll | Every mutual match stays reachable from the Buddies screen |
| Demo readiness | No data to match against | 20 seeded profiles available from the first login on Demo Night |

## 5. Scope

### In scope (8 weeks)

* A localized, privacy-first, platonic matchmaking responsive web application tailored exclusively to ASU, designed mobile-first.
* **Sign up and log in (Screen 1):**
  * The email must be an ASU student email: digits only before `@students.asu.edu.jo`.
  * Sign-up sets a Campus Buddy password (at least 8 characters), separate from the ASU portal password and stored only as a hash.
  * Simulated code verification at sign-up: students type a 6-digit mock code; no document or file is submitted, and no email is sent.
  * Log-in uses the email and the Campus Buddy password and skips the code step.
  * Sessions persist in browser LocalStorage, so a returning user on the same browser bypasses Screen 1.
* **Profile photos as text URL strings:**
  * Each profile stores one main photo (card front) and 1–3 extra photos (card back) as text URL strings pointing to publicly hosted images. At least one extra photo is required.
  * The app stores and renders the URL only. No image file is uploaded, stored, or hosted by Campus Buddy, so image hosting costs are zero.
  * Every submitted or edited photo URL passes a simulated Vision validation check before the profile can be saved, with a limit of 3 consecutive failed submissions per session.
* **Profile data:** required First Name, Last Name, Major, Academic Year, Gender, Gender Preference, Buddy Preference, WhatsApp number (international format), main photo URL, at least 1 extra photo URL, and at least 1 prompt answer; optional MBTI, Birthday (month and day only), up to 5 interest tags, preferred ASU landmark, up to 2 more extra photos and prompt answers, and social handles.
* **Curated prompts instead of bios:** students answer 1–3 prompts from a list of 20 (`prompts.md`), 150 characters each. There is no free-text "About me" field.
* **Same-gender default filter:** gender is required but never shown on a card. Each student matches with their own gender by default and can opt in to Both. The filter is mutual.
* **Social handles:** limited strictly to Instagram and Discord handles. No other social platforms are collected or displayed.
* **Profile editing:** users can return from the swipe deck to Screen 2 to edit their saved profile.
* **Daily discovery cap:** exactly 15 student profile cards per user per calendar day, counted from a log of viewed cards, to protect database health and give users a reason to return.
* **Block-based availability scheduling:** a 5×5 grid of standard blocks (8:30–10:00, 10:00–11:30, 11:30–1:00, 1:00–2:30, 2:30–4:00) across Sunday to Thursday. Users toggle the blocks they are free, and the deck is ordered by Overlap Score (e.g., "3 matching free windows").
* **ASU landmark selection:** a predefined list of campus landmarks (e.g., IT Faculty Lab, 360 Square, Main Library) shown on the card so mutual matches can decide where they'd prefer to meet.
* **Birthday tag:** a "B-DAY TODAY!!" tag on cards whose birthday matches the current day, without showing the date or age.
* **Card flip:** each card turns over in place to show the extra photos and prompt answers.
* **Zero-chat match gateway:** contact details stay locked until a mutual right-swipe, then surface immediately in the "Match Revealed" modal (Screen 4): full name, major, academic year, Instagram and Discord handles, and a WhatsApp link.
* **History Buddies (Screen 5):** a screen listing every mutual match with their WhatsApp and social buttons, reachable from the deck at any time, including after the daily cap is reached.
* **Seed data:** 20 pre-seeded mock profiles for development and Demo Night, plus seeded likes from selected mock profiles to the demo account so a mutual match can happen live.

### Out of scope

* Password resets (a code-verified reset is a v2 candidate), and any sign-in method other than email plus Campus Buddy password.
* Real code delivery by email (an automated email sender is a v2 candidate).
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
* Student account deletion or phone number removal. The honest reason is simplicity for a solo 8-week project.
* A free-text "About me" bio.
* Sharespot-style music, book and movie folders (v2 candidate; favourites are covered by prompts for now).
* Community reporting, flagging or auto-suspension (a 3-strike community flag is a v2 candidate).
* An opposite-gender-only matching option.

### Constraints

* **One developer:** a solo creator owning product architecture and codebase delivery.
* **Timeline:** a fixed 8-week delivery window under the IEEE WIE JS Mentorship Program, with one mentor sync per week and 3–8 hours of independent development between sessions.
* **One AI tool:** a single Claude Pro subscription for guidance and tutoring.
* **Zero financial cost:** free-tier hosting and infrastructure only; no paid image storage or paid runtime APIs.

## 6. Risks and Unknowns

| # | Risk or unknown | Why it matters | How it will be resolved |
| :- | :--- | :--- | :--- |
| 1 | Scope creep on the verification flow | Building automated email systems, document scanners, or an admin review dashboard would break the 8-week timeline. | Simulated code verification with the digits-only email format check and a 6-digit mock code held in local frontend state. Real verification is deferred until after the MVP. |
| 2 | Database strain from heavy usage | Uncapped swiping can exhaust free-tier database reads and writes during peak campus hours. | Enforce the 15-card daily cap at the query layer by counting the user's viewed-card records for the current calendar date. |
| 3 | Empty deck on Demo Night (cold start) | With no real users, the swipe deck, overlap scores, and match flow would have nothing to show. | Pre-seed 20 mock profiles with diverse ASU majors, interest tags, schedule blocks, landmarks, and public photo URLs, plus likes to the demo account so a mutual match can be shown live. |
| 4 | User hesitation to move to external apps | Introverted users may feel friction when leaving the platform to message someone on WhatsApp. | Use the profile structure to state explicit intent up front (e.g., "Java OOP study group at the IT lab," "morning coffee chat with someone from the AI majors"), giving students a clear reason to text. |
| 5 | Externally hosted photo URLs | Campus Buddy does not control the image behind a URL: it can break, change, or be replaced with inappropriate content after the profile is saved. | Validate each URL with the simulated Vision check on every submit or edit; accept that later changes to the external image are not re-checked in the MVP. |
| 6 | Data architecture scalability | Hard-coding campus parameters too early could make expanding to other universities difficult later. | Build ASU landmarks and schedule blocks as modular arrays in the data schema so they can accept new university inputs post-MVP. |
| 7 | Mock Security & LocalStorage Vulnerability | Anyone could theoretically insert a student alphanumeric ID string directly into browser LocalStorage to spoof an active session block. Because a single shared mock 6-digit code replaces SMTP verification, anyone who knows a peer's student number could sign up as them before they do. Log-in has no attempt limit, so passwords could be guessed. | Accepted as a standard prototype limitation for our solo zero-budget timeline. Passwords are stored only as salted hashes (N9), so a database leak does not reveal them. Student IDs are cryptographically random and unguessable. Production versions post-MVP will implement true independent SMTP closed-loop verification servers. |
| 8 | Render Free-Tier Cold Start Lag | Web instances sleep after 15 minutes idle, creating a 60-second lag on the next query. | The developer will manually open the public demo URL link exactly 5 minutes before the live presentation begins to pre-warm the instance container. |
| 9 | Used as a dating app | A campus swipe app can drift into dating, which breaks the platonic promise and makes students uncomfortable. | Product guardrails: the Screen 1 pledge, same-gender matching by default with a mutual filter, no free-text bios (curated prompts only), and cards anchored on schedule, major, buddy preference and campus landmarks. Community flagging is deferred to v2. |