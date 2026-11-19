# 10 Card flip and card back

**Why**
The front alone is too little to decide on; the back gives shy students photos and prompt answers to start a conversation with. (R14, N4, Screen 3)

**Done when**
* A flip button on the card turns it over in place with a 3D rotation of about 0.4s; with reduced motion enabled it swaps instantly.
* The back shows 3 photo slots down the left (filled from `mood_photo_urls`, empty ones as `#EFE9E0` placeholders), each prompt question with its answer beside the photos, interest tags, MBTI, and the landmark pill.
* A profile with 1 extra photo and 1 prompt shows 2 placeholder slots and keeps the layout.
* Content taller than the card scrolls inside the card; the flip button stays visible.
* Pass and Like work from either side; the flip button returns to the front.
* The back shows no last name, contact detail or gender.
* "See their card" in the Screen 4 modal shows the same back.

**Touches**
Card component on Screen 3 in `/public`, Screen 4 modal, deck route (read only)
