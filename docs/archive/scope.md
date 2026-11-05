Superseded by brief.md — kept for history.

# Scope Block

### The Size of the Job
* **Timeline:** Exactly 8 weeks, with 1 structured mentor sync session a week, and 3 to 8 hours of independent development between sessions.
* **Team Constraints:** Built entirely by 1 person acting as a solo full-stack developer.
* **Resource Constraints:** Utilizing exactly 1 AI tool subscription (Claude Pro) for guidance and tutoring with zero budget allocated for paid external hosting plans or premium backend tools.
* **Delivery Milestone:** Must run completely as a live, functional web application demo during the final Demo Night presentation.

# What is Out of Scope

To ensure a high-quality delivery within the 8-week timebox, the following features are deliberately left out of scope:
* **Native Mobile Apps:** The software will strictly be a web-based responsive architecture (no iOS/Android native development packages).
* **In-App Real-time Chat Sockets:** Building a continuous messaging channel is bypassed; users immediately transition to external WhatsApp/phone links upon a mutual match.
* **Unlimited Profile Swiping:** Capped at a strict loop of 15 cards per user per day to protect database processing efficiency and prevent user-base exhaustion, as well as provide a reason to keep visiting the web app again. 
* **Full SMTP Email Verification Server:** The system will check and validate that the registration input matches the university's `.edu` domain format, but it will skip executing live email verification loops to conserve development bandwidth.
* **Complex Multi-Campus Systems:** Limited exclusively to data fields, landmarks, and majors of the Applied Science Private University campus, rather any other campus space.
