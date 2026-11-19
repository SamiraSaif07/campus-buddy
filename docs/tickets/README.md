# Tickets

Thirteen tickets, numbered in build order. Each one assumes the ones before it exist. Every screen ticket includes its empty, loading and error states from `design.md`.

## Build first

1. **01 Project skeleton:** the server runs, the database connects, tests run.
2. **02 Database tables:** the three tables from `data-model.md`.
3. **03 Demo seed:** 20 profiles, including `demo.student@students.asu.edu.jo`, and the pre-liked swipes.

## The spine (one path all the way through)

4. **04 Sign-in gateway:** sign in as the demo account.
5. **05 Deck query:** the sorted, gender-filtered deck, with the cap and exhausted signals.
6. **06 Swipe card screen:** see the card fronts.
7. **07 Swipe recording:** swipe, count toward the cap, no duplicates.
8. **08 Match revealed modal:** a mutual like opens Screen 4.

After ticket 08, the demo works end to end with seeded data: sign in, swipe, match, open WhatsApp.

## Then

9. **09 History Buddies screen:** every match in one place.
10. **10 Card flip and card back:** photos and prompts on the back.
11. **11 Profile form and save:** new students create a profile; everyone can edit.
12. **12 Photo URL check:** server-side link check and the 3-strike lockout.

## Last (would be sad to cut, but could)

13. **13 Birthday tag trigger.**

If time runs short, cut in this order: ticket 13, then the flip animation in ticket 10 (keep an instant swap).
