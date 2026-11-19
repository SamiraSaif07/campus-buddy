# 11 Profile form and save

**Why**
New students need a profile before anyone can see them, and existing students need to edit it as their timetable changes. (R5, R6, R15, N5, N6, N8, Screen 2)

**Done when**
* Screen 2 shows every field in design.md, Screen 2, including 3 extra photo slots, 3 prompt slots with a "0/150" counter, the gender preference toggle, and the 5×5 grid with its counter.
* Gender preference appears after Gender is chosen, offers only the student's own gender or Both, and defaults to their own gender.
* The prompt picker offers the 20 prompts in `prompts.md` and greys out prompts already used; answers stop at 150 characters.
* Save is rejected, with each problem field marked, if any of the 11 required inputs is missing, the WhatsApp number is not digits-only international format, or any answer is over 150 characters; the server enforces the same rules.
* The first successful save creates the Student row with a random `STU_` id and the email from Screen 1, stores the id in LocalStorage, and routes to Screen 3.
* Opening Screen 2 from "Me" loads the saved values; saving overwrites them in place.
* There is no free-text "About me" field.
* Empty, loading and error states match design.md, Screen 2.

**Touches**
Screen 2 in `/public`, profile route in `/server/routes`, `students`
