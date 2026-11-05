# Technology Stack

## 1. Core Tooling Manifest
* Frontend Viewport: HTML5 / CSS3 / Vanilla JavaScript (Designed mobile-first, zero build-step compilation overhead)
* Backend Runtime: Node.js with Express framework (Single-language script execution environment)
* Database Layer: Managed PostgreSQL via Neon Free Plan (Compute sleeps after 5 minutes idle; data is safely kept persistently)
* App Hosting Tier: Render Hobby Workspace (Static frontend asset hosting and server route management from a single instance)
* Deployment: Render automatic repository deployment tracking is disabled. Production builds are triggered manually via Render incoming webhooks only when a Git release tag is verified.
* Code Testing Runner: Node.js Built-In Test Runner (`node --test` executed locally for automated query string validation)
* Core Utilities: `pg` library connection pool (native database driver), `dotenv` (local environment variables coordinator)
* Workspace Sandbox: GitHub Repository, VS Code Editor, Claude Pro Extension Tutor

## 2. Documented Stack Rejections & Trade-offs
* Rejected Frontend Frameworks (React/Vue): Avoided because the application requires only 4 responsive screens; vanilla code removes compilation overhead and web-pack debugging hours.
* Rejected Heavy Database ORMs (Prisma/Sequelize): Avoided to eliminate abstraction latency. Writing raw SQL strings with the native `pg` pool driver keeps queries clear, predictable, and simple to debug.
* Rejected Document Stores (MongoDB/Firebase): PostgreSQL chosen to execute relational inner joins natively at query time to derive mutual match states.
* Rejected Render Postgres Free Tier: Explicitly REJECTED because Render free databases expire after 30 days and are permanently deleted 14 days later, whereas Neon preserves data with no structural expiration date.
* Rejected WebSockets & SMTP Servers: WebSockets are fully supported by Render free tiers, but were rejected alongside mail servers because in-app text messaging is out of scope per N1 and registration verification is simulated.