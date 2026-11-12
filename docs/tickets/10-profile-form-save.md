# 10 Profile form and save

**Why**
New students need a profile before anyone can see them, and existing students need to edit their free blocks as their timetable changes. (R5, R6, N5, N6, Screen 2)

**Done when**
* Screen 2 shows every field in design.md, Screen 2, including the 5×5 grid with the "N free blocks selected" counter.
* Save is rejected, with each problem field marked, if any of the 7 required fields is missing or the WhatsApp number is not digits-only international format.
* The first successful save creates the Student row with a random `STU_` id and the email from Screen 1, stores the id in LocalStorage, and routes to Screen 3.
* Opening Screen 2 from Edit profile loads the saved values; saving overwrites them in place.
* Birthday is stored as `MM-DD` with no year; `schedule_blocks` is stored as 25 booleans matching the grid.
* Gender and MBTI default to blank; interest tags are added with Enter, up to 5.
* Empty, loading and error states match design.md, Screen 2.

**Touches**
Screen 2 in `/public`, profile route in `/server/routes`, `students`