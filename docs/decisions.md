# Decisions

## 2026-10-22 — Staging Dropped and Squash-Merge Adopted (`workflow.md`)
* **Context:** A solo developer cannot have a second reviewer approve PRs, and a separate staging environment adds hosting setup and upkeep with no team to use it.
* **Decision:** Drop the staging environment and keep only `dev` and `production`. Integrate every PR with Squash and Merge after a manual diff review in VS Code for leaked secrets.
* **Rejection:** Rejected a `staging` environment.
* **Consequence:** `main` history has one commit per feature or fix, and production deploys only from Git release tags.

## 2026-11-05 — Re-introduction of Profile Photos via URL Pointers
* **Context:** The initial project brief marked all image file uploads out-of-scope to adhere to strict zero-cost hosting limitations. However, a platonic matching deck lacks critical visual context and human connection without profile images.
* **Decision:** Re-introduce a main profile photo and up to 4 auxiliary mood photos into the profile suite.
* **Rejection:** Rejected direct binary image file uploads, asset payload handling, and custom cloud file hosting infrastructure.
* **Consequence:** Students will paste a text-string public image link URL into the profile manager. The application stores and renders this strictly as a standard text string data type, preserving a zero-cost infrastructure footprint.

## 2026-11-05 — Consolidation of Daily Cap Reset Logic
* **Context:** The old cap and reset requirements, now N3, featured overlapping mechanisms for managing the daily swipe cap limit.
* **Decision:** Consolidate into a single database runtime constraint. The card query logic counts logs matching the current calendar date stamp. 
* **Rejection:** Rejected a separate daily reset requirement driven by scheduled cron jobs or backend batch reset daemons.
* **Consequence:** N3 is the only cap rule; the count starts again each Asia/Amman calendar date with no reset job.

## 2026-11-05 — Unified Passwordless Sign-In / Registration Gateway
* **Context:** Screen 1 originally only accounted for new user registration onboarding, leaving returning users blocked with no system entry point.
* **Decision:** Consolidate Registration and Sign-in into a single gateway loop on Screen 1. If an email is recognized, the system routes the user straight to Screen 3 upon successful code simulation, bypassing Screen 2.
* **Rejection:** Rejected a separate login screen and password-based sign-in.
* **Consequence:** Eliminates the need to construct password fields, encryption helpers, or independent login screen components.

## 2026-11-05 — Profile Post-Save State Management
* **Context:** A user needs a path to toggle availability or update fields post-save, but multi-tab navigation introduces out-of-scope timeline creep.
* **Decision:** Profile saving is a persistent state override. Screen 3 will include a top navigation button to return to Screen 2 to toggle fields or schedule blocks. Editing profiles after submission is officially in scope via a persistent navigation button.
* **Rejection:** Rejected multi-tab navigation.
* **Consequence:** Screen 3 has a persistent navigation button back to Screen 2, and any edited photo URL re-runs the R3 check.

## 2026-11-05 — Vision Photo Check Stays Simulated
* **Context:** R3 requires every submitted or edited photo URL to pass a Vision validation check before save. A real vision API (e.g., Gemini Flash) costs money per call and needs an API key, which breaks the zero-cost constraint.
* **Decision:** Keep the Vision check simulated: it returns a `pass` or `fail` status and a rejection reason modeled on Gemini Flash, with no paid API call.
* **Rejection:** Rejected integrating a live paid vision API.
* **Consequence:** R3 and R4 (3-strike limiter) stay fully demonstrable at zero cost. Real image moderation is deferred to post-MVP, and Risk 5 (external images changing after save) remains open.

## 2026-11-05 — Buddy Preference Became Required
* **Context:** Buddy Preference (Study, Hangout, or Both) drives the R8 zero-overlap rule.
* **Decision:** Make Buddy Preference one of the six required profile inputs in R5.
* **Rejection:** Rejected keeping it optional.
* **Consequence:** Every card shows a stated intent, R8 can rely on the field always being present, and the profile cannot save without it.

## 2026-11-05 — Deck Filtering by Landmark or Buddy Preference Killed
* **Context:** `brief.md` promised filtering by landmark and buddy preference, but no requirement covered it.
* **Decision:** No deck filters. The deck is ordered only by R7 hierarchical sorting (overlap score, then random tie-breaker); landmark and buddy preference are displayed on the card, not used to filter.
* **Rejection:** Rejected landmark filters, buddy preference filters, and a general filter panel.
* **Consequence:** One query path to build and test. Filtering is listed as out of scope in `brief.md`.

## 2026-11-05 — Social Handles Limited to Instagram and Discord
* **Context:** The profile needed optional social links revealed after a mutual match.
* **Decision:** Collect and display only Instagram and Discord handles, alongside the required WhatsApp number.
* **Rejection:** Killed Twitter (X) and rejected any other social platforms.
* **Consequence:** R5, R11, and N4 cover exactly two optional handles. Other platforms are out of scope in `brief.md`.

## 2026-11-05 — Matches Drawer Added
* **Context:** The Match Revealed screen (Screen 4) shows contact details only once, at the moment of the match. After leaving it, a user had no way to reach an earlier match again, and nothing was reachable once the daily cap hit.
* **Decision:** Add a toggleable Matches Drawer on Screen 3 (R13) that lists every mutual match's first name with a WhatsApp action button, available even after the daily cap is reached.
* **Rejection:** Rejected a separate Matches screen or tab.
* **Consequence:** Mutual matches must be stored persistently, and N4 releases contact details through both R11 and R13.

## 2026-11-05 — Risk 7 (Shared Mock Code) Accepted
* **Context:** R2 uses a single shared 6-digit mock code for all accounts to avoid building an SMTP verification server. Anyone who knows a peer's ASU email could sign in as them and view their matches' WhatsApp links.
* **Decision:** Accept the risk for the MVP and Demo Night prototype, and record it as Risk 7 in `brief.md`.
* **Rejection:** Rejected building live SMTP email verification within the 8-week window.
* **Consequence:** The sign-in gate is a format check plus a simulated code, not real identity verification. Docs describe it as an ASU-email-gated sign-in, not a verified identity. Real closed-loop verification is post-MVP.
