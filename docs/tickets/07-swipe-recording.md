# 07 Swipe recording

**Why**
Each swipe has to count toward the daily cap and be the only swipe for that pair, or matches and the cap both break. (N3, N7)

**Done when**
* Pressing Pass or Like (or swiping) writes one `swipes` row (`like` or `pass`, random `SWP_` id) and one `daily_view_logs` row (random `LOG_` id), both with UTC timestamps.
* A second swipe by the same student on the same target is rejected and writes nothing; the screen skips that card silently.
* After 15 swipes on one Asia/Amman date, the next deck request returns the cap-reached signal; on the next date it returns cards again.
* If the save fails, the card slides back and shows "Your swipe didn't go through. Try again."
* The route response says whether the swipe completed a mutual match.
* `node --test` covers: the duplicate swipe is rejected; the 16th swipe in one day is refused.

**Touches**
Swipe route in `/server/routes`, Screen 3 buttons in `/public`, `swipes`, `daily_view_logs`, `/tests`