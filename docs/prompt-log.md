# Prompt Log (v1 to v4 Evolution)

### Version 1 (v1): The Naive Idea
* **Prompt:** "I want to build a campus social friend matching app like Tinder but for university students. Give me a full feature list, database design, and mobile app layout."
* **Why it changed:** The prompt was far too broad and broke the "Step One" protocol by asking the model to invent product features. It completely ignored problem-isolation and didn't account for the reality of a solo developer building an app in a tight 8-week cycle.

### Version 2 (v2): Isolate the Problem & Inspo
* **Prompt:** "I am choosing a project for an 8-week developer mentorship. Do not suggest code yet. Ask me one question at a time to interrogate the campus social isolation problem at Applied Science University (ASU). The app will be a platonic friend-matcher inspired directly by the Bumble BFF interface, utilizing a card-based swipe deck (People Tab) with a right-swipe to like and left-swipe to pass."
* **Why it changed:** Shifted to a focused interrogation strategy. This allowed me to map out how profiles should look (photos, short bios, and up to 5 customizable interest/lifestyle tags for icebreakers) while locking down the exact target audience: lonely or shy ASU undergraduates.

### Version 3 (v3): Introducing Privacy and Authentication Guardrails
* **Prompt:** "Let's refine the constraints. No contact data (WhatsApp/phone numbers) will be revealed until a mutual swipe right happens. Additionally, explore the idea of using ASU student emails (@students.asu.edu.jo) for authorization to ensure it remains a closed, verified campus network. How do we balance this authentication need with the 8-week timeline?"
* **Why it changed:** Addressed a critical product design question. While student email authorization is a non-negotiable priority for safety on a campus app, building a full email SMTP verification server from scratch is out of scope for a quick MVP. The prompt evolved to treat it as a hard constraint: the database will collect the `.edu` domain input to simulate student status, but full server-side verification loops will be put on hold to save engineering bandwidth.

### Version 4 (v4): Strict Database & Scope Bounding (Final)
* **Prompt:** "Apply final 8-week solo constraints using a single AI tool subscription through Claude Pro. Replace real-time chat sockets with an instant 'Match Revealed' screen that surfaces WhatsApp and optional social links if user provides them (ex: Instagram/Discord/Twitter). Cap daily profile views to 15 cards per user to protect database query loops from being exhausted instantly. Summarize the final engineering requirements for a web-based MVP."
* **Why it changed:** Locked down the exact technical boundaries. Stripping out the complex chat engine and instead resorting to available contacts, limiting the daily swipe loop, narrowing the campus down to one university as an MVP, and keeping it as a responsive web app rather than native mobile ensures a flawless, high-functioning live demo on final night.
