# 04 Sign-up and log-in

**Why**
Every visit starts here: new students sign up once with a code, and returning students log in with their Campus Buddy password. The pledge sets the platonic tone from the first second. (R1, R2, R12, N9, Screen 1)

**Done when**
* The welcome view shows the logo, the pledge, "Sign up with university email" and "Log in".
* Sign-up and log-in reject any email that is not digits-only before `@students.asu.edu.jo` with "Use your ASU student email, e.g. 202400000@students.asu.edu.jo.", checked on the server.
* Sign-up rejects a password under 8 characters, and an email that already has a Student row with "This email already has an account." and a link to log in.
* The code view shows six digit boxes, "Demo version: use the code from the Campus Buddy team." and Resend code (which sends nothing and says so). A code that does not match `MOCK_VERIFICATION_CODE` shows "That code is not correct. Try again." and clears the boxes.
* The correct code shows "You're in, ASUer. Let's set up your profile." and routes to Screen 2 with the email and password in frontend state only; nothing is written to the database and LocalStorage stays empty.
* Log-in with the right email and password saves `{"asu_matcher_student_id": "<id>"}` to LocalStorage and routes to Screen 3 with no code step. An unknown email and a wrong password both show "Incorrect email or password."
* No response ever contains `password_hash`, and no plain password is logged (N9).
* Opening the app with a stored id the server accepts goes straight to Screen 3; an id the server rejects clears LocalStorage and shows Screen 1.
* The empty, loading, error and full states of every view match design.md, Screen 1.

**Touches**
Screen 1 in `/public`, sign-up and log-in routes in `/server/routes`, a small password helper using `crypto.scrypt`, `students` table (read only)
