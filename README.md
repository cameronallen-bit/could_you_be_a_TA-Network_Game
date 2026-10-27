# Could You Be a Travel Advisor? (concept beta)

A three-minute phone assessment for the Avoya Travel Network. Twelve questions in three parts: how you'd work with travelers (five questions), the trips you'd love to plan (three), and the life you want to build (four). There are no wrong answers. Every pick shows a short insight about the job.

The result is an advisor blueprint:

- **A style:** The Curator, The Planner, The Storyteller, The Problem-Solver or The Connector, plus a second style.
- **A specialty:** ocean cruises, river cruises, honeymoons and romance, luxury and bucket list, families and groups, or tours and culture.
- **The life lines:** when and where they'd work, why they'd do it, and what they'd leave behind.

Below the blueprint: how the Avoya Network fits each part of it, how the network works and what it costs, then an optional two-step form for the recruiting team.

Built on The Price Is Fright's play-anytime engine (commit 79c9039): the REST store, the retry queue and the share card. One self-contained `index.html`, no build step, no dependencies (the admin page loads the Firebase SDK; the ad pixel loads only if an ID is set).

## Links

| What | Address |
| --- | --- |
| The assessment | https://cameronallen-bit.github.io/could_you_be_a_TA-Network_Game/ |
| Pilot link (staff, friends and family; counted) | `…/?c=pilot` |
| Advisor link (working Avoya advisors; kept out of the player counts) | `…/?c=advisor` |
| Test mode (real database, nothing counts, leads marked test) | `…/?test=1` |
| Demo mode (nothing leaves the browser) | `…/?demo=1` (add `&seed=150` to preview the style shares and answer counts) |
| Admin page | `…/?role=admin` (demo data: `…/?role=admin&demo=1`) |
| Share previews | `…/r/curator.html`, `planner`, `storyteller`, `problem-solver`, `connector` |

The five older previews in `r/` (`natural-closer` and the rest) are from the first beta. They still open the assessment.

## What shows when

- **"N% of players chose this too"** under a pick: once 50 people have finished.
- **"N% of players share your style"** on the blueprint: once 100 people have finished. The admin page can change the 100.
- **"Among working Avoya advisors who've taken this assessment, the most common style is…"**: once 30 people have finished from the advisor link.
- Only each device's first finish counts. Retakes, the advisor link and test mode don't.

## Setup still to do

1. **Database rules.**
   - In the Firebase console, open project `price-is-fright`, then Realtime Database, then Rules.
   - Inside `"rules"`, beside the `priceIsFright` block (and Choc Trivia's), paste the `"advisorQuiz": { … }` block from `database.rules.json`. Leave out its two root lines (`".read": false` and `".write": false`).
   - Publish. Until then the assessment works, but counters, plays and leads are refused. Each device keeps its play and lead and retries later.
2. **Admin sign-in.**
   - In Authentication, open Sign-in method, then Google, then Enable.
   - In Settings, then Authorized domains, add `cameronallen-bit.github.io`.
   - Open `?role=admin`, sign in and copy your user ID from the page. In the database, add `advisorQuiz/admins/<your user ID>` = `true`.
3. **Before ads:**
   - The network team signs off on the network steps, the "how the network fits" lines and the form promises. Those promises are the guide, "within one business day" and the advisor follow-up.
   - Legal signs off on the consent wording and the privacy coverage.
   - The paid team picks the ad category and the Meta Pixel ID (`ANALYTICS.metaPixelId`, empty now).
   - Move to an Avoya-owned Firebase project and an Avoya address.

## Files

- `index.html`: the assessment and the admin page. The config is at the top of the script: `FIREBASE_CONFIG`, `QUIZ`, `BANK_VERSION` and `BANKS` (the questions), `STYLES`, `SPECIALTIES`, `FITS`, `NETWORK_STEPS`, `FORM`, `LEAD_SINK` and `ANALYTICS`.
- `r/*.html`: the share previews. Each has its own title and image, then forwards to the assessment.
- `img/`: photos, the six tile thumbnails (`t-*.jpg`) and the share images. Also `BRAND.md` (colors and type from AvoyaNetwork.com) and `SOURCES.md` (photo licenses).
- `database.rules.json`: the rules.

## Updating

Edit `index.html`: the questions are in `BANKS`, the result copy is in `STYLES`, `SPECIALTIES` and `FITS`. Then in this repo choose Add file, then Upload files, drop it in and commit. Pages redeploys in about a minute.

A wording edit that keeps an option's meaning is just an edit. A new question, or an option that changes meaning, bumps `BANK_VERSION`, which starts fresh counters.
