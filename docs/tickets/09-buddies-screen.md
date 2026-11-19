# 09 History Buddies screen

**Why**
The match modal shows only once; without one place to find every buddy again, a closed modal means a lost contact. (R13, R11, N4, Screen 5)

**Done when**
* The Buddies tab opens Screen 5, which lists every mutual match (two opposite `like` swipes), newest first.
* Each row shows main photo, full name and major on the left, and WhatsApp, Instagram and Discord buttons on the right (Instagram and Discord only if present); WhatsApp opens `https://wa.me/<whatsapp_number>`.
* Tapping a row opens the Screen 4 modal for that buddy, headed with their first name, with "Back to Buddies".
* The Buddies tab works from the Daily Cap Reached and Deck Exhausted states, and the Deck tab returns to Screen 3.
* The list survives a page reload and a new sign-in.
* The empty, loading and error states match design.md, Screen 5.

**Touches**
Screen 5 in `/public`, buddies route in `/server/routes`, Screen 4 modal (reused), `students`, `swipes` (read only)
