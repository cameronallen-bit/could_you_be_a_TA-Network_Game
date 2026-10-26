# Could You Be a Travel Advisor? (concept beta)

A three-minute phone quiz for the Avoya Travel Network: three trips (a Caribbean cruise, an Alaska cruise, a Danube river cruise) and the Big Booking, ten items in all. Guess the price, work out the commission, answer a destination question. Every reveal teaches one true thing about the business. The result is an advisor type, a score out of 10,000 and, once 100 people have played, an honest percentile. Below it: how the Avoya Network works, then an optional two-step form for the recruiting team.

Built on The Price Is Fright's play-anytime engine (commit 79c9039): the REST store, the retry queue, the guess box, the drum reveal, `closeness()` and the share card. One self-contained `index.html`, no build step, no dependencies (the admin page loads the Firebase SDK; the ad pixel loads only if an ID is set).

## Links

| What | Address |
| --- | --- |
| The quiz | https://cameronallen-bit.github.io/could_you_be_a_TA-Network_Game/ |
| Pilot link (staff, friends and family; counted) | `…/?c=pilot` |
| Advisor link (working Avoya advisors; kept out of the percentile) | `…/?c=advisor` |
| Test mode (real database, nothing counts, leads marked test) | `…/?test=1` |
| Demo mode (nothing leaves the browser) | `…/?demo=1` (add `&seed=150` to preview a percentile) |
| Admin page | `…/?role=admin` (demo data: `…/?role=admin&demo=1`) |
| Share previews | `…/r/natural-closer.html`, `deal-hunter`, `destination-nerd`, `full-package`, `explorer` |

## Setup still to do

1. **Database rules.** Firebase console, project `price-is-fright`, Realtime Database, Rules. Inside `"rules"`, beside the `priceIsFright` block (and Choc Trivia's), paste the `"advisorQuiz": { … }` block from `database.rules.json`. Leave out its two root lines (`".read": false`, `".write": false`). Publish. Until then the quiz plays fine, but counters, plays and leads are refused (each player's device keeps its play and lead and retries later).
2. **Admin sign-in.** Authentication, Sign-in method, Google, Enable. Then Settings, Authorized domains, add `cameronallen-bit.github.io`. Open `?role=admin`, sign in, copy your user ID from the page, and in the database add `advisorQuiz/admins/<your user ID>` = `true`.
3. **Before ads:** the network team's sign-off on the steps and commission examples (items 8 and 10 are drafts), legal's on the consent wording and privacy coverage, the ad category, and the Meta Pixel ID (`ANALYTICS.metaPixelId`, empty now). Move to an Avoya-owned Firebase project and an Avoya address.

## Files

- `index.html`: the quiz and the admin page. The config is at the top of the script: `FIREBASE_CONFIG`, `QUIZ`, `BANK_VERSION` and `BANKS`, `TYPES`, `NETWORK_STEPS`, `FORM`, `LEAD_SINK`, `ANALYTICS`.
- `r/*.html`: the five share previews (each has its own title and image, then forwards to the quiz).
- `img/`: item photos, share images, `BRAND.md` (colors and type from AvoyaNetwork.com) and `SOURCES.md` (photo licenses).
- `database.rules.json`: the rules.

## Updating

Edit `index.html` (prices live in `BANKS`), then in this repo: Add file, Upload files, drop it in, commit. Pages redeploys in about a minute. A price change on the same sailing is an edit; a new item or sailing bumps `BANK_VERSION`, which starts fresh counters.
