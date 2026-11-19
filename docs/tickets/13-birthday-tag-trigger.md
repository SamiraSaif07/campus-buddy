# 13 Birthday tag trigger

**Why**
A small, friendly icebreaker on the card front, without ever showing a date or age. (R10, N5)

**Done when**
* A card whose `birthday` equals today's month and day in Asia/Amman shows "🍰 B-DAY TODAY!!" beside the first name on the front.
* A card with any other birthday, or none, shows no tag.
* At 00:00 Asia/Amman the tag moves to the new day's birthdays with no reset job.
* No screen shows a birthday date, birth year or age.
* `node --test` covers: a matching `MM-DD` shows the tag and a non-matching one does not.

**Touches**
Deck route in `/server/routes`, card front on Screen 3 in `/public`, `/tests`
