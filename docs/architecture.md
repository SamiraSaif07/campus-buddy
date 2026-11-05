# Architecture

The application implements a mobile-first, three-tier web architecture optimized for a solo developer workflow, where the client browser interacts exclusively with a unified Node/Express server application.

## 1. Data Routing Map
* Layer 1: Client Browser Viewport (HTML5 / Vanilla JavaScript Asset Layer)
* Layer 2: Web Server Router (Node.js / Express API Coordinator)
* Layer 3: Relational Database Storage (PostgreSQL Persistent Data Store)

## 2. Layer Responsibilities & System Boundaries

### A. The Client Browser (Frontend Viewport Layout)
* **Core Responsibilities:**
  * Extracts the cryptographically random student `id` text token from LocalStorage (`{"asu_matcher_student_id": "STU_XXXXX"}`) on network actions for passwordless session persistence.
  * Manages the visual toggle display states between the Pending vs Verified onboarding screens entirely in frontend state.
  * Tracks photo URL verification failures locally to manage the 3-strike lockout and the 15-minute countdown timer.
* **Not Allowed To Do (Browser Boundaries):**
  * The browser is strictly forbidden from querying or communicating with the database layer directly.
  * The browser never calculates the daily exploration cap count, checks for mutual matching conditions, or evaluates schedule overlap metrics. It only displays what the server returns.
  * *Accepted Security Gap:* Because the 3-strike lockout lives in local frontend memory, a browser refresh will reset the timer. This is an accepted prototype constraint for our solo timebox.

### B. The Web Server (Node/Express API Coordinator)
* **Core Responsibilities:**
  * Serves the static HTML/CSS/JS frontend files directly to the client from a unified public folder asset layer, eliminating complex cross-domain hosting dependencies.
  * **The Session Check:** Validates incoming student `id` tokens against existing database rows to accept returning user connections.
  * **Simulated Vision Check:** Executes server-side formatting checks to return a simulated pass/fail JSON grade payload modeled on Gemini Flash criteria, preventing client console overrides.
  * **Registration Onboarding (R1, R12):** Enforces the `@students.asu.edu.jo` email format restriction and handles account routing rules.
  * **Profile Form Validation (R5):** Rejects saves if any of the 7 required inputs are missing or if the WhatsApp string violates the international numeric structure.
  * **Deck Retrieval Layer (R7, R8):** Pulls candidate datasets by filtering out the active user's own profile and anyone they have already swiped, dynamic sorting by schedule block overlap at runtime, and returning an empty-deck signal payload so the client browser can draw the Deck Exhausted view state if availability hits zero.
  * **Match Evaluation Engine (R11, R13):** Detects if opposite right-swipe entries exist between two IDs to confirm mutual matches, returning the match's contact details for Screen 4 and the Screen 3 Matches Drawer.
  * **Timezone Normalization (R10, N3):** Stores all data logs in UTC format. Converts timestamps dynamically to Asia/Amman time only when counting the daily cap and checking birthdays.
  * **Swipe Recording (N3, N7):** Writes the swipe operation and its corresponding daily view log row in UTC format, and rejects duplicate swipe attempts by the same student on the same target profile.
  * **Session Rejection:** An unrecognized or cleared student ID string is rejected by the server query, causing the frontend state to return the viewport to Screen 1.
* **Not Allowed To Do (Server Boundaries):**
  * The server is strictly prohibited from sending last names, phone numbers, or social handles inside standard Screen 3 swipe deck card data payloads.

### C. The Database Layer (Isolated Persistent Storage)
* **Core Responsibilities:**
  * Safely persists and indexes records within the structural relational schemas (`students`, `daily_view_logs`, `swipes`).
  * Integrates the 20 pre-seeded student profiles and pre-liked swipe entries on initial database seeding to guarantee live matching flows on Demo Night.
* **Not Allowed To Do (Database Boundaries):**
  * The database layer contains zero hard or soft row deletion routines. All profile modifications execute as direct text string overwrites in place for solo project simplicity.