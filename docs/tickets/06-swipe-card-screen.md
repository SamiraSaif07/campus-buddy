# 06 Swipe card screen

**Why**
This is where students spend their time; the front has to show the right fields and nothing private. (R9, N4, N5, Screen 3)

**Done when**
* Screen 3 shows one card at a time from the deck route. The front shows the main photo, first name, Overlap Score badge (or the R8 notice), major, academic year and buddy preference, and nothing else.
* No last name, contact detail, gender, birthday date or age appears anywhere on the screen.
* A main photo that fails to load shows a grey box with the first name's initial; the rest of the card still renders.
* The cap-reached and deck-exhausted signals show the Daily Cap Reached and Deck Exhausted messages from design.md.
* The top bar has "Me" (to Screen 2), the app name, and "Buddies" with a count badge (to Screen 5).
* Colours and contrast follow the visual system in design.md; loading and error states match design.md, Screen 3.

**Touches**
Screen 3 in `/public`, deck route (read only)
