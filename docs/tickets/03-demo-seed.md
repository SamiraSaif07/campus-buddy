# 03 Demo seed

**Why**
With no real users, the deck and the match flow would be empty on Demo Night. The seed is what makes the demo work. (N2, Risk 3)

**Done when**
* Running the seed script inserts exactly 20 Student rows with random `STU_` ids, diverse majors, academic years, interest tags, schedule blocks, landmarks and public `https` photo URLs ending in an image extension.
* One of the 20 is the demo account, `samira.sweidan@students.asu.edu.jo`.
* At least 3 other seeded profiles have a `like` Swipe (random `SWP_` id) aimed at the demo account.
* At least one seeded profile has 0 overlapping blocks with the demo account but shares its major or buddy preference, so R8 can be shown.
* Running the seed twice still leaves exactly 20 Students and no duplicate Swipes.

**Touches**
`/seed` script, dev Neon database