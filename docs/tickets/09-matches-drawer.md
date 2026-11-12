# 09 Matches drawer

**Why**
Screen 4 shows only once; without the drawer a closed modal means a lost match. (R13)

**Done when**
* The Matches button on Screen 3 opens a drawer listing every mutual match (two opposite `like` swipes) with first name and a working WhatsApp button.
* The button's badge shows the number of matches.
* The drawer opens and works on the Daily Cap Reached and Deck Exhausted states too.
* With no matches it shows "No matches yet. When someone you liked likes you back, they'll appear here."
* Loading and error states match design.md, Screen 3, Matches Drawer.

**Touches**
Matches Drawer in `/public`, matches route in `/server/routes`, `students`, `swipes` (read only)