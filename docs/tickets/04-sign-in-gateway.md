# 04 Sign-in gateway

**Why**
Every visit starts here: new students register and returning students get back in, with no passwords. The pledge sets the platonic tone from the first second. (R1, R2, R12, Screen 1)

**Done when**
* The pledge line "Strictly for platonic study buddies and campus friends at ASU." is visible in every state.
* An email not ending in `@students.asu.edu.jo` shows the R1 error banner and goes no further.
* A valid email switches to the Pending view; a code that does not match `MOCK_VERIFICATION_CODE` shows "That code is not correct. Try again." and stays in Pending.
* Correct code + new email: routes to Screen 2 with the email held in frontend state; nothing is written to the database and LocalStorage stays empty.
* Correct code + recognized email: saves `{"asu_matcher_student_id": "<id>"}` to LocalStorage and routes to Screen 3.
* Opening the app with a stored id the server accepts goes straight to Screen 3; an id the server rejects clears LocalStorage and shows Screen 1.
* The empty, loading, error and full states match design.md, Screen 1.

**Touches**
Screen 1 in `/public`, sign-in route in `/server/routes`, `students` table (read only)
