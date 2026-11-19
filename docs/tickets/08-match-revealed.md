# 08 Match revealed modal

**Why**
The match is the moment the app exists for: two students who both said yes get each other's contact details. (R11, N1, N4, Screen 4)

**Done when**
* A right-swipe that completes a mutual match opens the Screen 4 modal over Screen 3 with a coral backdrop, "It's a match!", main photo, first and last name, major, academic year, Instagram and Discord (only if present), and an Open WhatsApp button linking to `https://wa.me/<whatsapp_number>`.
* A one-sided right-swipe opens nothing and reveals no contact details.
* Right-swiping a seeded profile that already liked the demo account opens the modal.
* There is no message box or chat anywhere in the modal.
* "Continue matching" closes the modal and shows the next card.
* Loading and error states match design.md, Screen 4.

**Touches**
Screen 4 modal in `/public`, match details route in `/server/routes`, `students`, `swipes` (read only)
