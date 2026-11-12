# 06 Swipe card screen

**Why**
This is where students spend their time; it has to show the right fields and nothing private. (R9, N4, N5, Screen 3)

**Done when**
* Screen 3 shows one card at a time from the deck route with first name, photos, major, academic year, gender, buddy preference, interest tags, MBTI, landmark and the Overlap Score badge, or the R8 notice when it applies.
* Empty optional fields show no row or placeholder.
* No last name, contact detail, birthday date or age appears anywhere on the screen.
* A photo URL that fails to load shows a grey box with the first name's initial; the rest of the card still renders.
* The cap-reached and deck-exhausted signals show the Daily Cap Reached and Deck Exhausted messages from design.md.
* The top bar has the Matches button and the Edit profile button (Edit goes to Screen 2).
* Loading and error states match design.md, Screen 3.

**Touches**
Screen 3 in `/public`, deck route (read only)