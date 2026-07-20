# Design & Theme (current site — keep consistent)

The existing site (`Genesis-Pool-and-Spa-main/`) has an established visual identity. Any
new work (including the contact popup below) should match it rather than introduce a new
style.

**Positioning**: "The Chemistry of Clean" — the brand leans on pool science/water safety
credibility (FUN, HEALTH, SAFE, CLEAN) rather than a luxury/resort spa vibe. Copy reads as
a licensed, trustworthy local technician, not a lifestyle brand.

**Color palette** (all defined as CSS custom properties in `css/styles.css`):
- Primary blue: `#1565C0`, dark `#0D47A1`, light `#42A5F5`, pale tint `#E3F2FD`
- Deep navy accent: `#0D2A6B` (used in hero gradients and dark text-on-light contexts)
- Amber/gold accent: `#FFB300` (used sparingly — badges, the "What's Included" banner
  — as the one warm contrast note against all the blue)
- Neutrals: white, gray-50 `#F8FAFB` through gray-800 `#1E293B`, body text `#334155`
- Hero and other feature sections use diagonal blue gradients (e.g.
  `linear-gradient(160deg, #0D2A6B 0%, #1565C0 60%, #1976D2 100%)`), not flat fills.

**Typography**: Poppins (system-ui/-apple-system fallback), sans-serif throughout.
- Headings are bold-to-heavy (700–800 weight), often with tight/negative letter-spacing
  at large sizes (hero h1 uses `clamp(2.75rem, 6.5vw, 5.5rem)` with `-0.025em` tracking)
- Small labels/eyebrows (hero label, section labels, badges) use uppercase, wide letter
  spacing (~0.14em), small size (~0.7rem), bold weight — a consistent "kicker text" pattern
  above every major heading
- Section headings scale fluidly with `clamp()` rather than fixed breakpoints

**Shape & elevation**:
- Rounded corners everywhere: 12px standard radius, 20px for larger cards/panels
- Soft, blue-tinted shadows rather than neutral gray (`rgba(21,101,192,0.10)` and
  `0.16)` for the elevated/hover state) — shadows reinforce the blue theme instead of
  looking like a generic gray drop shadow
- 0.25s ease transitions on interactive elements (buttons, header state, nav links)

**Header behavior**: transparent/white-text over the hero, then flips to a white
background with dark text once scrolled (`#header.scrolled`) — a common pattern for
sites with a full-bleed hero image.

**Hero**: full-bleed photo background (currently hosted on Wix's CDN) with a dark
navy-to-transparent gradient overlay for text legibility, big bold headline with one
word highlighted in a color span, short supporting line, then CTA buttons (solid
primary-blue button + a ghost/outline button with translucent white border).

**Page structure/rhythm** (`index.html`): Hero → Features → "What's Included" banner
(amber, informational, no CTA) → Services → Before/After results → About → Service Area
(dark blue `.light` section-header variant) → Testimonials → Contact. Each major section
repeats the same header pattern: small uppercase eyebrow label, bold heading, then
centered supporting paragraph.

**Takeaway for future work**: reuse the existing CSS variables (`--primary`,
`--primary-dark`, `--primary-xl`, `--gray-*`, `--radius`, `--shadow`), Poppins font,
uppercase-eyebrow + bold-heading section pattern, and blue-gradient/rounded-card
treatment. Any new UI (e.g. the contact popup) should look like it was built by the same
designer as the rest of the site — blue-and-white, rounded, soft blue shadows, amber used
only as a rare accent.

---

# Services List

Current services on the live site (`index.html` → `#services`, one `.service-card` each)
— reflected in the footer services list and the contact form's service dropdown on both
`index.html` and `schedule.html`:
- Weekly Pool & Spa Cleaning
- Chemical Balancing & Water Testing
- Equipment Repair & Maintenance
- Tile Cleaning

**Decision: Spa & Hot Tub Service merged into Weekly Pool Cleaning (no longer a standalone
card).** Per direct instruction, the standalone "Spa & Hot Tub Service" card was removed
and its bullets — spa draining & cleaning, jet & filter maintenance, cover & equipment
care — were folded into what's now **"Weekly Pool & Spa Cleaning"** (its 4th original
bullet, "water chemistry balancing," was dropped as a duplicate of the separate Chemical
Balancing service). Card count went from 5 back to 4; the About section's "Core Services"
stat and the contact-form service dropdown were updated to match. The Chemical Balancing
& Water Testing service card's icon was also changed from the ruler-like lines to a
beaker/flask icon, matching the 🧪 test-tube motif used in the promo bar.

**Free tile cleaning for new customers — revised framing (decision superseded)**:
originally scoped as a "free to anyone who signs up this summer (2026)" limited-time
promo. That framing was **replaced** per direct instruction: one free tile cleaning is
now presented as an ongoing, standing part of what new customers get with regular
service — not a seasonal/limited-time offer, and with no "schedule now" CTA attached to
it. It's called out in the Tile Cleaning service card's feature list ("One free tile
cleaning for new customers") and in the [[What's Included Banner]] below, not as its own
promotional banner. Flagging this reversal explicitly in case "limited time" was meant to
stay separate from the "ongoing bundle" framing — correct if that's wrong.

---

# Copy Change: Promo Bar ("Free Inspection" → "Free Chemical Balancing + Cleaning")

**Status: done.** "FREE Inspection + Testing" and its 🔍 magnifying-glass icon
(`&#128269;`) were retired everywhere on the site. New copy: **"FREE Chemical Balancing +
Cleaning"**, with a 🧪 test-tube icon (`&#129514;`) in place of the magnifying glass, to
match the "Chemistry of Clean" positioning (see [[Design & Theme]]).

Applied across every occurrence, per the "rename everywhere" decision:
- `index.html` and `schedule.html` promo bars
- `schedule.html` `<title>`, meta description, OG/Twitter tags, page `<h1>`, hero
  supporting paragraph (rewritten to match the new promise — balancing + cleaning, not an
  equipment inspection), hidden form `_subject` field, and submit button copy
- `thankyou.html` confirmation copy

Left unchanged on purpose (not part of this rename): the `schedule.html` form's address
placeholder, the schema.org `addressLocality` field, and testimonial authors' cities —
none of those are brand copy, so they weren't touched.

**Also done — promo bar no longer shows a phone number**: "Call 916-251-4798" was
removed from both promo bars. The header's own call button already surfaces the phone
number, so the promo bar now only shows the offer link.

**Promo bar link — tried in-page anchor, reverted back to `schedule.html`.** Briefly
changed to scroll to an on-page form instead of navigating away (added `id="request-form"`
to `schedule.html`'s form section and `section[id] { scroll-margin-top: 100px; }` in
`css/styles.css` for the anchor landing spot — that CSS rule is harmless and stays even
though it's not load-bearing for the promo bar anymore). **Final decision: both
`index.html` and `schedule.html` promo bars link to `schedule.html`** — a dedicated page
was fine; what actually needed work was rewriting that page's content, which was already
done as part of the "Free Inspection" rename above.

**Also done — "Get a Free Quote" retired**: the hero's outline button now reads
**"Schedule a Free Cleaning & Chemical Balance"** and links to `schedule.html` (this
button was not the "top banner," so it kept its page-navigation link rather than
becoming an anchor).

---

# Area Code Rebrand: "Sacramento" / "916" → "916/530"

**Status: done.** Genesis now also services the 530 area code, so brand copy that
previously said "Sacramento" or "916" alone was updated to **"916/530"** across
`index.html`, `schedule.html`, and `thankyou.html`: title tags, meta/OG/Twitter
descriptions, hero label, about-section badge and stat, service-area heading/intro
paragraph, and footer taglines.

Deliberately **not** changed (these are data/attribution, not brand messaging):
- The favicon's baked-in "916" text (all three pages) — the SVG is a tiny 100×100 badge
  at a large font size; "916/530" would overflow or force the text illegibly small.
  Flagged rather than shipping a broken icon; open question if a different favicon
  treatment (e.g. a logo mark instead of digits) is wanted instead.
- `schedule.html` form address placeholder ("1234 Main St, Sacramento, CA") — needs a
  realistic example city, not an area-code pair.
- The schema.org `addressLocality` field (`index.html` JSON-LD) — schema.org expects an
  actual city name for structured data/Google Business matching; "916/530" isn't a valid
  locality value.
- Testimonial authors' cities (e.g. "J. Martinez, Sacramento, CA") — real customer
  attribution, not business branding.

---

# What's Included Banner (replaces "Free Equipment Repair" banner)

**Status: done — copy finalized.** Genesis does not offer free equipment repair — repair
pricing varies by job (this is already accurately represented by the "Equipment Repair &
Maintenance" service card, which was never marketed as free). The old amber banner
claiming "FREE Equipment Repair" with a "Limited Time Offer" tag and a "Schedule Now"
button has been replaced with an informational banner, same visual slot/amber styling,
framed explicitly as **part of the regular service bundle you get upon signing up** —
not a promo, not something to schedule separately:
- Tag: "Included With Every Signup"
- Heading: "What You Get When You Sign Up"
- Copy: "Regular service includes weekly cleaning and chemical balancing, plus one free
  tile cleaning and ongoing brush & net replacement — all part of the bundle. Need a new
  net or brush? We'll provide them. Equipment repair is available too, with pricing based
  on the job."
- No "Limited Time Offer" tag, no CTA/schedule button.
- CSS classes renamed accordingly: `.free-repair-banner` → `.included-banner`,
  `.free-repair-inner` → `.included-inner`, `.free-repair-tag` → `.included-tag`; the old
  button-related CSS (`.free-repair-btn` and its hover/mobile rules) was deleted since
  there's no button anymore. Layout changed from a two-column flex (text + button) to a
  single centered text block.

**Consistency pass across the rest of the site** (needed once the bundle framing was
finalized, since other sections still described the old free-inspection/free-repair
framing):
- `index.html` Weekly Pool Cleaning service card gained a "Free brush & net replacement"
  bullet, so the card list matches what the banner promises.
- `schedule.html`'s "What to Expect" grid had a leftover **"Equipment Check"** card
  promising "we inspect your pump, filter, heater... for issues" — a free-diagnostic
  promise left over from the original "Free Inspection" offer, which directly
  contradicted "we don't offer free equipment repair/diagnosis." Replaced with **"We've
  Got You Covered"**: "Need a new net or brush? We'll provide them — plus a free tile
  cleaning if you're a new customer," with a gift-box icon instead of the wrench icon.

**Now links to the contact form (decision reversed from "no CTA")**: the banner gained
`id="included"` and a **"Sign Up for the Bundle"** button linking to `#contact`. The
Tile Cleaning service card's "One free tile cleaning for new customers" bullet became a
link, **"Free Tile Cleaning with Signup,"** pointing at `#included` — so the flow is
Tile Cleaning card → What's Included banner → Sign Up button → contact form. Both the
banner button and (if reused elsewhere) any link with a `data-preselect-service`
attribute auto-select that value in the contact form's service dropdown via a small
`main.js` handler, so clicking "Sign Up for the Bundle" lands on the contact form with
**"Bundle (Signup) — Cleaning + Balancing + Free Tile Cleaning"** already selected. A new
`bundle` option was added to that dropdown for this purpose.

**"Free tile cleaning upon signup" added to both promo bars.** `index.html` and
`schedule.html` both now show "Free tile cleaning upon **signup**" next to the main promo
link (hidden on mobile at the same breakpoints the old phone number used to be, via a new
`.promo-extra` class, to keep the bar to one line). The word "signup" is the link, and it
needs to pre-select the bundle option same as the banner button above — but
`schedule.html` has no `#contact` form of its own, so the same-page `data-preselect-service`
click handler doesn't work there. Solved with a **URL query parameter**: `schedule.html`'s
"signup" link points to `index.html?service=bundle#contact`; `main.js` now also reads a
`service` query param on page load (`applyServicePreselect(new
URLSearchParams(window.location.search).get('service'))`) and applies it the same way the
click handler does, so both the same-page and cross-page paths share one function.
`index.html`'s own "signup" link stays same-page (`href="#contact"
data-preselect-service="bundle"`), since it doesn't need the URL-param path.

**Contact form fields loosened**: Email Address and Message are no longer `required` on
the `#contactForm` (index.html) — labels updated to say "(optional)" so it's visually
clear. Note: with Email and Message both optional, and Phone already optional, the only
required fields left are First/Last Name — technically someone could submit with no
contact info at all. Flagging since that's a real tradeoff of "make it optional," not
something to silently paper over.

---

# Testimonials — placeholder, real reviews pending

**Status: done.** The three fake/placeholder testimonials (J. Martinez, S. Kim, T.
Robinson) were removed from `index.html`'s Reviews section — they were never real
customer quotes and shouldn't ship as if they were. Replaced with a single dashed-border
"Customer reviews coming soon — check back after our first completed jobs" placeholder
(new `.testimonials-placeholder` CSS), rather than leaving empty/fake cards. The original
3-column `.testimonials-grid` / `.testimonial-card` CSS was left in place (unused for
now) so real reviews can be dropped back into that same layout later without rebuilding
the styling.

---

# Contact Preference Popup — Lead Routing Plan

Architecture notes for the planned popup: "How would you like us to reach you?" with three
independent checkboxes — visitors can select any one, two, or all three.

- [ ] Talk to one of us (call)
- [ ] Have us email you
- [ ] Text me

The **Call** and **Text** channels both run on Twilio — full detail on the SMS
confirm-flow, the Twilio number, and phone-number verification now lives in
[`plan/twilio.md`](twilio.md) rather than here, since that file collects everything
Twilio-specific in one place.

## Email — "have us email you"

- **Goal**: an AI-drafted reply is sent from `contact@genesispoolandspa.com`.
- **Mechanism**: webhook → Zapier → AI step (Zapier's built-in AI action, or a Code step
  calling an LLM API) drafts a reply grounded in the visitor's message and the business's
  services/hours/service-area info → sent via Gmail/Google Workspace integration
  authenticated as `contact@genesispoolandspa.com`.
- **Dependency**: that mailbox needs to be a real inbox Zapier can authenticate against
  (Gmail/Workspace OAuth, or SMTP credentials if using a different provider).

## All three selected

Each flow fires independently and in parallel — no sequencing or priority logic needed.

## Open questions before building

1. Which LLM/API to use for email drafting.
2. Popup trigger timing — on load, exit-intent, or after a delay.
3. (Twilio/Zapier/SMS-specific open questions — including the confirmation SMS flow,
   phone verification, and Zapier plan tier — live in `plan/twilio.md`.)

## Status

Architecture only — not yet built for the popup UI or the email flow. The Google Sheets
lead-logging piece is further along:
- The Sheet exists ("Genesis Pool and Spa - Booking Requests").
- The Apps Script (`doPost`) now logs submissions from **both** forms on the site, not
  just `schedule.html` — a **"Form Source"** column (`"Schedule Form"` vs. `"Contact
  Form"`) tags which one was used, since the `index.html` `#contact` "Send Us a Message"
  form previously didn't submit anywhere at all (it only faked a local success message).
  A **"Service Interested In"** column was also added for that form's service dropdown.
  The script self-heals the header row on `doPost` (checks row 1 against the expected
  headers and rewrites it if different), so it also upgrades the sheet's original
  13-column header to the new 15-column one automatically the first time it runs — no
  manual cell editing needed.
- `js/main.js` and `schedule.html` are wired client-side to POST to the Apps Script's
  deployed URL: the contact form via `fetch(..., {mode:'no-cors'})` (no navigation, so a
  normal fetch is fine), the schedule form via `navigator.sendBeacon()` (chosen over
  fetch specifically because that form does navigate away afterward, to FormSubmit.co →
  `thankyou.html` — sendBeacon guarantees the request survives the navigation, an
  in-flight fetch might not).
- Still blocked on: the user deploying the Apps Script as a Web App and sending back the
  `/exec` URL — until then, `window.APPS_SCRIPT_URL` in `js/main.js` is a placeholder
  (`'REPLACE_WITH_DEPLOYED_APPS_SCRIPT_URL'`) and both forms' logging calls are no-ops by
  design (guarded so they don't fire against a fake URL).

The email flow (Zapier + Gmail) is blocked on Gmail/Workspace access for
`contact@genesispoolandspa.com`. The Twilio/Zapier SMS pieces (Call, Text, phone
verification) are tracked separately in `plan/twilio.md`.
