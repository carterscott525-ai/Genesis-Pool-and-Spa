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

**Also done — promo bar link now scrolls to the on-page form instead of navigating to a
new page**: previously the promo bar linked to `schedule.html` from every page (a full
page navigation, even redundantly reloading `schedule.html` from itself). Now:
- `index.html` promo bar → `href="#contact"` (the existing "We're Here to Help" contact
  form section on the same page)
- `schedule.html` promo bar → `href="#request-form"` (added `id="request-form"` to its
  own `.sched-form-section`)
- Added `section[id] { scroll-margin-top: 100px; }` in `css/styles.css` so anchored
  sections don't land underneath the fixed header.
- Scope note: only the promo bar link itself was changed. Other buttons that still point
  to `schedule.html` (hero's "Schedule a Free Cleaning & Chemical Balance", the thank-you
  page's "Book Another Appointment") were left alone since only "the top banner redirect
  link" was called out — flag if those should also become in-page anchors.

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

---

# Contact Preference Popup — Lead Routing Plan

Architecture notes for the planned popup: "How would you like us to reach you?" with three
independent checkboxes — visitors can select any one, two, or all three.

- [ ] Talk to one of us (call)
- [ ] Have us email you
- [ ] Text me

Twilio (+1 916-251-4798, already live on the site) is scoped to the AI SMS channel only —
it is not used for the call-alert or email flows.

## Call — "talk to one of us"

- **Goal**: get the lead on a live phone call with a sales representative, either almost
  immediately or at a time the lead picks — via a Twilio-driven warm transfer, not a
  direct ring to staff.
- **Form options**: a lead who chooses "talk to one of us" picks between:
  - **ASAP** — Twilio calls them back, likely within 1–2 minutes of submitting.
  - **Scheduled** — a specific date/time they choose, any day, between **6:00 AM and
    7:00 PM**.
- **Twilio call flow** (the lead is never connected straight to a sales rep — Twilio
  always screens the call first):
  1. At the requested time (immediately, or at the scheduled moment), Twilio places an
     outbound call to the lead's number.
  2. The flow **confirms a live human connection** before anything else happens — e.g. a
     "Press 1 to speak with a Genesis Pool and Spa representative" IVR prompt is more
     reliable here than relying solely on Twilio's Answering Machine Detection (AMD),
     which has known false-positive/latency issues; this step exists specifically to
     make sure a person, not voicemail, picked up.
  3. Once confirmed, notify the manager (voice call, SMS, or Slack/push via a webhook)
     that a lead is connected and will be redirected to a sales rep **in 2 minutes** —
     a heads-up buffer so the rep isn't caught by an unannounced call.
  4. The lead is held for that 2-minute window (hold message/music — e.g. "Thanks!
     Connecting you with a specialist now.").
  5. After the buffer, Twilio bridges/conferences the lead's call into the sales
     representative's line.
- **Caveat**: the <2-minute intent only covers the ASAP option's initial outbound dial —
  the 2-minute manager heads-up is a deliberate buffer *after* the lead is already
  confirmed live on the phone, separate from that callback-speed goal. Reliability still
  depends on a sales rep actually being reachable when the bridge happens; consider what
  happens on no answer (see open questions).

### Twilio + Google Sheets — what's needed to build the redirect flow

**Twilio side**:
- A Twilio number with **Programmable Voice** enabled (may be the existing
  916-251-4798 number or a separate one — ties into the open question below about
  splitting voice from the AI SMS channel).
- A call-flow definition — either a **Twilio Studio Flow** (no-code: Trigger → Gather/
  confirm → notify manager → Wait 2 min → Dial/Conference) or a small **serverless
  function** (Vercel/Netlify/etc.) driving the Voice API + TwiML directly, which gives
  more control over retries and no-answer branching.
- The "confirm connection" step: a `<Gather>` press-1 IVR prompt (recommended) or AMD,
  wired into whichever flow engine is chosen above.
- A **conference resource** (Twilio `<Conference>`) to bridge the two legs: the lead
  joins on confirmation, the sales rep's leg is added after the 2-minute wait.
- A trigger for *when* the call fires:
  - ASAP: fire immediately on form submission.
  - Scheduled: Twilio has no built-in delayed-call scheduler, so this needs an external
    scheduler — a cron-triggered serverless function, or a Zapier "Schedule" step —
    that checks pending scheduled times and fires the Twilio API call when one arrives.
- `statusCallback` webhooks on each call leg so the system knows whether the lead
  answered/confirmed, and whether the rep picked up — this is what feeds status back
  into the sheet below.

**Google Sheets side**:
- The sheet acts as the lead/schedule database: one row per form submission (name,
  phone, contact preference, ASAP vs. scheduled time, timestamp, status, assigned rep).
- Form → Sheets connection: either the form posts directly via the Sheets API, or (same
  pattern as the rest of this plan) Zapier sits in between — form submits → Zapier →
  appends a row → Zapier/the serverless function reads that row to trigger Twilio.
- A service account or OAuth credential for whichever tool (Zapier or the serverless
  function) needs to read/write the sheet.
- Status gets written back to the same row as the call progresses (called / no-answer /
  connected / redirected to rep X) so the sheet doubles as a lightweight lead-tracking
  log the manager can glance at.
- Scheduled times should be stored unambiguously with timezone, since the "6 AM–7 PM"
  window is presumably Sacramento local time.

## Email — "have us email you"

- **Goal**: an AI-drafted reply is sent from `contact@genesispoolandspa.com`.
- **Mechanism**: webhook → Zapier → AI step (Zapier's built-in AI action, or a Code step
  calling an LLM API) drafts a reply grounded in the visitor's message and the business's
  services/hours/service-area info → sent via Gmail/Google Workspace integration
  authenticated as `contact@genesispoolandspa.com`.
- **Dependency**: that mailbox needs to be a real inbox Zapier can authenticate against
  (Gmail/Workspace OAuth, or SMTP credentials if using a different provider).

## Text — AI SMS

- **Goal**: the Twilio number carries on a live SMS conversation, with an AI agent
  answering visitor questions.
- **Mechanism**: Twilio Programmable Messaging number → inbound SMS webhook → small
  backend (serverless function, e.g. a Vercel/Netlify function, or Zapier + Code step) →
  AI reply grounded in business info → response sent back via the Twilio API.
- This is the one channel that actually needs Twilio — a live two-way AI conversation loop
  isn't something Zapier alone handles cleanly.
- Open question: does this same number also carry live voice calls (forwarded to staff),
  or should voice ring a separate line from the AI SMS number?

## All three selected

Each flow fires independently and in parallel — no sequencing or priority logic needed.

## Open questions before building

1. Zapier plan tier — needs instant (webhook-based) triggers, not polling, to hit the
   sub-minute call-alert goal.
2. Who/what receives the "call now" alert — a specific phone, a rotation, or a shared
   Slack/push channel?
3. Which LLM/API to use for email drafting and SMS replies.
4. Whether +1 916-251-4798 carries both live voice and AI SMS, or if those should split.
5. Popup trigger timing — on load, exit-intent, or after a delay.
6. Which sales rep gets the redirect — a fixed number, a rotation, or availability-based
   — and where that logic lives (Sheets lookup vs. hardcoded in the Twilio flow).
7. What happens on no-answer/no confirmation, or if no rep is reachable after the
   2-minute buffer — retry, voicemail, or fall back to a callback request?
8. Who is "the manager" for the 2-minute heads-up — a specific phone/Slack channel, or
   a rotation of their own?
9. Twilio Studio Flow (no-code) vs. a custom serverless function for the confirm →
   notify → wait → bridge logic — Studio is faster to stand up, a function gives more
   control over retries/no-answer handling.

## Status

Architecture only — not yet built. Blocked on: Twilio account/number confirmation, a
Zapier account (tier depends on the speed requirement above), Gmail/Workspace access for
`contact@genesispoolandspa.com`, and the open questions above.
