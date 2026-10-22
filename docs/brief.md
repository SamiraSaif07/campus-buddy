# Project Brief: Campus Buddy (ASU Matcher)

## 1. The Problem

First-year and introverted undergraduates at Applied Science Private University (ASU) experience severe social anxiety, isolation, and institutional friction. The sheer scale of the campus makes finding compatible study or social peers intimidating.

Today, students try to bridge this gap through massive, unstructured 500+ member course WhatsApp groups. In these groups, peer-to-peer study plans and genuine attempts at social connection get lost instantly under a wall of chat noise. There is also no structured way for students to filter potential peers by compatible schedule windows, shared academic courses, or specific campus landmarks.

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

* Register with an email that passes the exact ASU student format check (`@students.asu.edu.jo`), then enter a mock university registration code string to complete simulated verification.
* See the app respond to their verification status: an unverified account (`Is_Verified` = false) is held at a pending-verification state, while a verified account (`Is_Verified` = true) reaches the swipe deck.
* View a curated stack of text-based student profile cards showing major, student year, buddy preference (Study, Hangout, or Both), up to 5 interest tags, schedule overlap, and preferred ASU landmark.
* Filter potential matches by timetable block overlap, chosen ASU landmark, and buddy preference.
* Swipe right (like) or left (pass), and on a mutual right-swipe, instantly see the other student's WhatsApp/phone number and optional social handles on a dedicated "Match Revealed" screen.

**Demo data:** The database will be pre-seeded with 20 mock profile records covering diverse ASU majors, interest tags, schedule blocks, and landmarks. This bypasses the cold-start problem so the swipe deck, overlap scores, and match flow are fully demonstrable during the live Demo Night presentation.

### Measures

| Measure | Baseline (today) | Target (week 8) |
| :--- | :--- | :--- |
| Time to discover a compatible peer | Hours spent filtering crowded WhatsApp chats | Minutes within a single, intentional swipe session |
| Pre-meeting communication friction | High anxiety over open-ended small talk | Immediate hand-off to direct WhatsApp with shared context (courses, free blocks, landmark) |
| Daily profile card consumption | Infinite, unstructured scrolling | Capped at exactly 15 deliberate cards per user per day |
| Closed campus network | Unmoderated public phone numbers | Every account passes the `@students.asu.edu.jo` format check and the simulated registration-code verification step |
| Demo readiness | No data to match against | 20 seeded profiles available from the first login on Demo Night |

## 5. Scope

### In scope (8 weeks)

* A localized, privacy-first, platonic matchmaking responsive web application tailored exclusively to ASU.
* **Simulated student verification:**
  * Registration validates that the email exactly matches the `@students.asu.edu.jo` format.
  * Students type a mock university registration code string into a text prompt; no image or file is uploaded.
  * Verification status is a purely local frontend state toggle (`Is_Verified` true/false), and the UI renders a distinct state for each value.
* **Daily discovery cap:** exactly 15 student profile cards per user per day, to protect database health and give users a reason to return.
* **Block-based availability scheduling:** the university day is divided into standard blocks (e.g., 8:30–10:00, 10:00–11:30, 11:30–1:00, 1:00–2:30, 2:30–4:00). Users toggle the blocks they are free, and the app calculates a simple Overlap Score (e.g., "3 matching free windows") showing how easy it will be to meet up.
* **ASU landmark selection:** a predefined list of campus landmarks (e.g., IT Faculty Lab, 360 Square, Main Library) embedded in the card structure so mutual matches can decide where they'd prefer to meet.
* **Zero-chat match gateway:** contact details stay locked until a mutual right-swipe, then surface immediately on the "Match Revealed" screen.
* **Seed data:** 20 pre-seeded mock profiles for development and Demo Night.

### Out of scope

* Any admin dashboard, manual review queue, or file verification workflow.
* All image uploads, including profile photos and student ID photos.
* Live automated SMTP email verification servers.
* AI-driven OCR document scanning for student verification (a possible post-MVP direction).
* Checking registration codes against real university records.
* In-app text chat sockets, real-time message synchronization, or thread management.
* Unlimited profile swiping.
* Multi-campus deployment or cross-university database indexing.
* Native iOS or Android mobile application builds.

### Constraints

* **One developer:** a solo creator owning product architecture and codebase delivery.
* **Timeline:** a fixed 8-week delivery window under the IEEE WIE JS Mentorship Program, with one mentor sync per week and 3–8 hours of independent development between sessions.
* **One AI tool:** a single Claude Pro subscription for guidance and tutoring.
* **Zero financial cost:** free-tier hosting and infrastructure only.

## 6. Risks and Unknowns

| # | Risk or unknown | Why it matters | How it will be resolved |
| :- | :--- | :--- | :--- |
| 1 | Scope creep on the verification flow | Building automated email systems, image upload pipelines, photo scanners, or an admin review dashboard would break the 8-week timeline. | Simulate verification with the exact email format check, a typed mock registration code string, and a local frontend `Is_Verified` toggle. Real verification is deferred until after the MVP. |
| 2 | Database strain from heavy usage | Uncapped swiping can exhaust free-tier database reads and writes during peak campus hours. | Enforce the 15-card daily cap directly at the query logic layer. |
| 3 | Empty deck on Demo Night (cold start) | With no real users, the swipe deck, overlap scores, and match flow would have nothing to show. | Pre-seed 20 mock profiles with diverse ASU majors, interest tags, schedule blocks, and landmarks. |
| 4 | User hesitation to move to external apps | Introverted users may feel friction when leaving the platform to message someone on WhatsApp. | Use the profile structure to state explicit intent up front (e.g., "Java OOP study group at the IT lab," "morning coffee chat with someone from the AI majors"), giving students a clear reason to text. |
| 5 | Text-only cards may feel less personal | Dropping photos removes a familiar visual cue from Bumble BFF-style cards. | Lean on major, buddy preference, interest tags, schedule overlap, and landmark as the icebreakers; revisit photos after the MVP. |
| 6 | Data architecture scalability | Hard-coding campus parameters too early could make expanding to other universities difficult later. | Build ASU landmarks and schedule blocks as modular arrays in the data schema so they can accept new university inputs post-MVP. |