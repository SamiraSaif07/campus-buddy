# Tickets

Twelve tickets, in build order. Build them in this order: each one assumes the ones before it exist.

## Build first

1. **01 Project skeleton:** the server runs, the database connects, tests run.
2. **02 Database tables:** the three tables from data-model.md.
3. **03 Demo seed:** 20 profiles, including the demo account, and the pre-liked swipes.

## The spine (one path all the way through)

4. **04 Sign-in gateway:** sign in as the demo account.
5. **05 Deck query:** the sorted deck, with the cap and exhausted signals.
6. **06 Swipe card screen:** see the cards.
7. **07 Swipe recording:** swipe, count toward the cap, no duplicates.
8. **08 Match revealed:** a mutual like opens Screen 4.

After ticket 08, the demo works end to end with seeded data: sign in, swipe, match, open WhatsApp.

## Then

9. **09 Matches drawer:** reopen earlier matches.
10. **10 Profile form and save:** new students create a profile; everyone can edit.
11. **11 Photo URL check:** server-side link check and the 3-strike lockout.

## Last (would be sad to cut, but could)

12. **12 Birthday tag trigger.**

Each screen ticket includes its empty, loading and error states from design.md.