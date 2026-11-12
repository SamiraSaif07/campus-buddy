# 11 Photo URL check

**Why**
Photos are only links, so the server must reject bad links before they reach other students' decks, and the browser must not be able to skip it. (R3, R4, N6)

**Done when**
* The server rejects a photo URL that does not start with `https://` or does not end in `.jpg`, `.jpeg`, `.png` or `.webp`, returning the exact reason, and Screen 2 shows that reason.
* Saving is blocked while any photo URL is failed or still being checked, including when a saved URL is edited.
* After 3 consecutive failures, photo inputs are disabled for 15 minutes with a live countdown and the campus compliance message.
* A URL that passes shows its preview; a preview that fails to load shows "Image can't be loaded".
* There is no file upload input anywhere in the app.

**Touches**
Photo fields on Screen 2 in `/public`, photo check route in `/server/routes`