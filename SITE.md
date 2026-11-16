# draftera.ca site map

How the site is built and where everything lives, so any change can be made from a plain request. Read this before editing.

## Branches

| Branch | Use |
| --- | --- |
| `nonprofit-redesign` | Working branch. All edits happen here. |
| `(root)` | Live branch. GitHub Pages publishes draftera.ca from it. Never commit or push here without Ken's explicit yes (use the `publish-site` skill). Quote it in Git: `git checkout "(root)"`. |

Do not touch `CNAME` (it holds `draftera.ca`), GitHub Pages settings or DNS. "Enforce HTTPS" is on (Ken, 2026-10-13), so http:// addresses forward to https://. If it ever turns off, Ken re-ticks it in the repository's Settings > Pages (only the kenjhop-gif account sees Settings).

## Preview

From `website/`:

```
python -m http.server 8000
```

Then open http://localhost:8000/. Links and assets use root paths (`/assets/...`), so preview from `website/` itself, not a parent folder. Check pages at 375px and 1440px wide: no sideways scrolling.

## How the site is built

- Plain HTML and CSS. No build step and no JavaScript apart from the Google Analytics snippet (the `application/ld+json` blocks are data, not scripts).
- `_config.yml` only tells GitHub Pages not to publish this file (`SITE.md`). Add any other internal notes to its `exclude` list.
- `assets/css/tokens.css`: design tokens (colours, type, spacing, radii, rules), copied from `brand/tokens.css`. The bottom block, "Site additions", holds a few values the brand file doesn't name yet.
- Stylesheet links carry a version (`?v=20261110`). Bump it on every page when CSS changes, so browsers and GitHub Pages fetch the new styles.
- Section spacing is the `--section-y` token (tightened 2026-11-10 to about 77px between sections at desktop width, at Ken's request).
- `assets/css/site.css`: all layout and components. It uses only `var(--...)` tokens for colours. Never hard-code a colour in it; add or change a token instead.
- Font: General Sans, loaded from the Fontshare CDN. Decided 2026-09-30 to keep it there: the ITF Free Font Licence allows self-hosting for your own site but forbids making the files available through a publicly accessible repository, and this GitHub Pages repo is public. Fontshare stays listed in the privacy notice.
- Logo: `assets/img/wordmark.svg` (light backgrounds) and `assets/img/wordmark-reversed.svg` (Green or Ink backgrounds), copied from `brand/logo/`. All are outlined paths (no font needed), generated 2026-09-30 from General Sans with kerning. In the header and footer the wordmark is inlined as `<svg class="wordmark">` with two paths; their colours come from CSS (`.wm-draft`, `.wm-era`).
- Favicon: `assets/img/favicon.svg` (a Paper "D" on Green, drawn as a path), plus `favicon-32.png` (older browsers) and `apple-touch-icon.png` (180px, full-bleed Green; also the schema.org logo).
- Share image: `assets/img/share-card.png` (1200 x 630: wordmark, Paper background, the Home headline), set as `og:image` and `twitter:image` on every page with card type `summary_large_image` by `machine_data.py`. Source to re-render it: `brand/share-card/share-card.html` (headless Chrome screenshot at 1200 x 630).
- Decision points (round 2, 2026-11-16): the starters sit in a Mist band (`.sec--band`) on white cards with a 3px Green top edge (`.starter-card`; `.starter-panel` on Services and For partners), each with one audience pill (`.pill`: "For one organization" or "For networks and funders"). Prices and step numbers are Green. Second prices are labelled (`<span class="price-alt-label">Half-day:</span> $2,500`). No other colours, no badges.
- Design reference: `brand/design-system.md` and `brand/directions/final/` (`index-portrait.html`, `about.html` and the offer-page direction).
- Copy source: the HTML itself is the source of truth for wording. `brand/copy/<page>.md` is generated from it (`python .claude/skills/edit-site/export_copy.py`, run from the project root after every change). `brand/website-copy.md` is retired.

## Section markers

Every section on every page is wrapped in comment markers, and the `<section>` element carries the same id:

```html
<!-- section:home-hero -->
<section class="hero" id="home-hero">...</section>
<!-- /section:home-hero -->
```

To change a section, search for `section:<id>`. Section ids are stable; keep them when you edit. A new section gets a new marker pair and id in the pattern `<page>-<name>`.

Numbered sections show a small green number (`<p class="label">01</p>`). If you add, remove or reorder sections, renumber the labels on that page.

## Shared parts (repeated on every page: update all 10 pages together)

| Part | Marker | Notes |
| --- | --- | --- |
| Fonts link | `<!-- shared:fonts -->` in `<head>` | Fontshare stylesheet for General Sans |
| Analytics | `<!-- shared:analytics -->` in `<head>` | Google Analytics GA4 `G-L787TEY7BD`, Google signals and ad personalization off. Must stay on every page. |
| Machine-readable data | `<!-- shared:machine -->` in `<head>` | PNG icon links, Open Graph tags and schema.org JSON-LD (Draftera as ProfessionalService on Home, Ken as Person on About, each offer as a Service with CAD prices, FAQPage from each page's `<details>`). Generated; never edit by hand. Run `python .claude/skills/edit-site/machine_data.py` from the project root after any change (prices are hard-coded in that script). No JSON-LD on Privacy or 404. |
| Header | `section:site-header` | Wordmark, nav (Services, Workflow Build, For partners, About Ken; changed 2026-11-15), "Book a conversation" button (to `/conversation.html`). Only `aria-current="page"` differs per page. Under 900px the nav drops to a second row. |
| CTA band | `section:cta-band` | Green block, heading, one paragraph that differs per page, button "Book a 30-minute conversation" (the one call-to-action wording everywhere except the header button, 2026-11-15) to `/conversation.html#booking-embed` (lands straight on the Calendly calendar; every booking button on the site, including the header button, uses this link), then the email backup line. `conversation.html` has no CTA band since round 2: the calendar sits at the top (section `book`). On `partners.html` the band reads "Talk about a workshop for your members or grantees" with the button "Book a conversation about a workshop" (Ken, round 2). On `index.html` it also contains `<span id="contact">` so old `#contact` links still land. |
| Email backup line | `<p class="email-alt">` | "Prefer email? Write to hello@draftera.ca." (mailto link) directly under the hero booking buttons and in every CTA band. Small secondary text; never a second button. Not in the header, and not under the in-section booking buttons added in round 2 (under the Home starter cards and at the end of each Services starter). |
| Footer | `section:site-footer` | Wordmark, tagline, links (Home, Services, Workflow Build, Embedded AI Partnership, For partners, Draftera Foundations, About Ken, Book a conversation to `/conversation.html#booking-embed`, Privacy), location, hello@draftera.ca, copyright. Only `aria-current` differs. |

Each page also has its own `<title>`, meta description and canonical link in `<head>`.

## Pages and sections

Exact wording lives in the HTML. `brand/copy/<page>.md` is generated from it by `.claude/skills/edit-site/export_copy.py`; run it after every change. Facts and offers must match `CLAUDE.md`. Updated 2026-11-15 (Ken's change brief: three fixed-price starters, Home cut down, Services rebuilt, new menu) and 2026-11-16 (change brief round 2: colour at decision points, shorter pages, calendar at the top of the booking page, share image). Old section ids that other sites may link to are kept as empty `<span class="svc-anchor" id="...">` landing points.

### Home: `index.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `home-hero` | | Eyebrow, headline "Practical AI, built with your team." (h1, "your team" in Green), lede, booking button with "Free. 30 minutes. No obligation." under it (`hero-primary`), "See the services", partner link ("Network, association or funder? See partner options."), email line, portrait card linking to About ("Former executive director, School Lunch Association. Tripled revenue, 34 school locations, 80+ staff.") |
| `home-starters` | 01 | "Three ways to start" in the Mist band: three white cards (`ladder ladder--cards ladder--three`, `starter-card`) with number, pill, one line, link, Green price and labelled second price; booking button; hidden comment slot for a future client quote (show nothing until Ken supplies an approved one); Partnership line ("starts at about two days a month"); CAD and taxes note |
| `home-situation` | 02 | Staff already use AI without shared guidelines (one short paragraph); Imagine Canada statistic with source as a callout |
| `home-ladder` | 03 | "How we work together": three steps (`flow flow--three`): free conversation, a starter, ongoing support |
| `home-principles` | 04 | Responsible, practical, measured (one sentence each), then one line linking to Draftera Foundations |
| `cta-band` | | Shared CTA |

### Draftera Foundations (the framework): `foundations.html`

Always pair the name with its subtitle on first use on a page: "a framework for using AI in nonprofits and social enterprises" (no "well", Ken 2026-11-10). Draftera prepares organizations for independent verification; it never certifies, accredits or approves. Never "AI Ready". The framework "lines up with" or "is being mapped to" national standards, never "compliant with".

| Section id | Label | Purpose |
| --- | --- | --- |
| `framework-hero` | | Eyebrow "The framework", h1 "Draftera Foundations", subtitle, opening line (the method behind every service) |
| `framework-purpose` | 01 | Why it exists; Imagine Canada figures (80% of all nonprofits use AI and 10% of all have a formal policy; 64% of the nonprofits that use AI have none and aren't developing one; bases confirmed by Ken 2026-11-16) |
| `framework-who` | 02 | Who it is for |
| `framework-pieces` | 03 | The diagram (`fw`, each card links to its row), the six descriptions in summary order (`gets fw-pieces`, ids `piece-leadership`, `piece-data`, `piece-oversight`, `piece-transparency`, `piece-people`, `piece-value`), then inventory and annual review on its own Mist panel (`fw-hold`, label "What holds the six together", id `piece-inventory`); each row ends with a `fw-starter` line linking to the starter that puts it in place |
| `framework-evidence` | 04 | International frameworks, Canadian standards and guidance, sector evidence (no mapping-in-progress note since 2026-11-15) |
| `framework-use` | 05 | Start with one piece, build the rest in stages, seek independent verification if desired (`flow flow--three`) |
| `framework-principles` | 06 | People stay responsible, plain language, documented evidence, independence |
| `framework-scope` | 07 | Not legal advice; "Draftera Foundations is reviewed annually." (no version label since 2026-11-15); link to Services |
| `cta-band` | | Shared CTA (heading and button) |

### Services: `services.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `services-hero` | | Eyebrow "Services and prices", h1, lede (three fixed-price ways to start), intro naming the framework |
| `services-policy` | 01 | Mist band, white card: AI policy and staff session, $1,950 (h2 id `ai-policy-staff-session`), pill "For one organization". Labelled facts (`dl.svc-facts`; colons allowed here, Ken 2026-11-15): who it is for, what you get, how long, also available, what comes next (links to `workflow-build.html`); booking button. Old ids `ai-policy`, `training`, `data-privacy-review`, `transparency-statement` land here |
| `services-workshop` | 02 | Mist band, white card: Group workshop, $1,500, "Half-day: $2,500" (h2 id `group-workshop`), pill "For networks and funders"; booking button |
| `services-workflow` | 03 | Mist band, white card: Workflow Build, $2,500, "Two workflows: $4,000" (h2 id `workflow-build`), pill "For one organization", two-workflow sentence, link to the Workflow Build page; booking button |
| `services-ongoing` | | Ongoing support (unnumbered since round 2): the Partnership, link (old id `foundations-check-up` lands here) |
| `services-start` | | How to start (unnumbered): free conversation, written fixed-price proposal; CAD and taxes note; CAN/DGSI 101 named in full; Digital Governance Council fee (old id `full-foundations` lands here) |
| `cta-band` | | Shared CTA |

### Workflow Build: `workflow-build.html`

The former Draftera AI Foundations Engagement page, moved and renamed 2026-11-10. Styles in `pages/workflow-build.css`.

| Section id | Label | Purpose |
| --- | --- | --- |
| `workflow-hero` | | Eyebrow "Workflow Build", headline (full stop outside the emphasis), lede, framework line, booking button, price card ($2,500 one workflow, $4,000 two), fee note under the card (`workflow-fee-conditions`: fixed price in a written proposal, CAD and taxes, licences paid by the client), delivered-by line |
| `workflow-problem` | 01 | The work that crowds out the mission (three sentences) |
| `workflow-who` | 02 | Who it is for, numbered rows |
| `workflow-gets` | 03 | What the fee includes, then a closing line on what the organization keeps (old id `workflow-outcomes` lands here) |
| `workflow-how` | 04 | Weeks 1 to 3; note with staff time (2 to 4 hours per person) |
| `workflow-workflows` | 05 | Typical workflows I build, note linking to the booking page, then the illustrative example (old id `workflow-example`; `[TODO` comment marks it for a real case study later) |
| `workflow-data` | 06 | Data protection statement |
| `workflow-faq` | 07 | Common questions (`details`), no Partnership mention (old id `workflow-price` lands here) |
| `workflow-next` | | Stone strip: the only Partnership mention on the page, link |
| `cta-band` | | Shared CTA |

### For partners: `partners.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `partners-hero` | | Eyebrow "Partner work", h1 "For partners", subtitle, opening paragraph (first step is a group workshop) |
| `partners-workshop` | 01 | Mist band, white card: start with a group workshop, pill "For networks and funders", $1,500, "Half-day: $2,500", what is included, CAD note, link to Services, sample agenda table (`agenda`; timings a draft, `TODO (Ken)` comment) |
| `partners-need` | 02 | The need (Imagine Canada figures and barriers), first person |
| `partners-next` | 03 | What can follow a workshop, without prices. Old ids `partners-networks`, `partners-funders` land here |
| `partners-sector` | 04 | Sector and standards bodies: practitioner's view, guidance, verification explained (Digital Governance Council, own fee; I prepare, never certify), link to Foundations |
| `partners-framework` | 05 | The framework behind the work ("My work is organized around..."), link |
| `cta-band` | | "Talk about a workshop for your members or grantees", work beyond the workshop is scoped to fit, button "Book a conversation about a workshop" (old id `partners-talk` lands just above) |


### Draftera Embedded AI Partnership: `partnership.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `partnership-hero` | | "An ongoing partner for AI and operations" (full stop outside the emphasis), lede, two sentences on why ongoing support matters, booking button, price card (Core, Plus, scope lines; no minimum term) |
| `partnership-does` | 01 | Intro with the framework (first mention, with subtitle), six areas of support; Reporting holds the quarterly board report list (colon before the list, Ken 2026-11-16). Old ids `partnership-why`, `partnership-accountable` land here |
| `partnership-levels` | 02 | Core vs Plus table (`compare`): fee, scope, typical time (Core about two days a month, Plus about four days a month), minimum term, suited to, review, quarterly report |
| `partnership-rhythm` | 03 | Monthly rhythm (review, build, train, report), then a paragraph on a typical first three months (old id `partnership-first-quarter`) |
| `partnership-operations` | 04 | Stone block: operational challenges beyond AI |
| `partnership-terms` | 05 | Minimum term, after that, prices and taxes |
| `partnership-fit` | 06 | Who the Partnership is for |
| `partnership-faq` | 07 | Common questions (no repeats of the sections above), then an About Ken link (old id `partnership-who`) |
| `cta-band` | | Shared CTA |

### About Ken: `about.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `about-hero` | | Headline "Over two decades of leading organizations...", lede, 4:5 portrait |
| `about-sla` | 01 | School Lunch Association, seven years; proof numbers (3x revenue, 34 locations, 1 million+ meals, 80+ people) |
| `about-university` | 02 | "Leading operations in a large institution": operational leadership in a large Canadian university. Never say Ken works there now, never name the title or Memorial |
| `about-why` | 03 | Why AI, and why nonprofits and social enterprises; hands-on AI work; ends with "I developed Draftera Foundations…", linked to the framework page |
| `about-principles` | 04 | The principles behind every engagement |
| `about-credentials` | 05 | Education and credentials |
| `about-where` | 06 | Location line plus remote working |
| `cta-band` | | Shared CTA |

### Book a conversation: `conversation.html`

The free first step is the **30-minute conversation** (Google Meet, booked through Calendly at https://calendly.com/hello-draftera/30min, email as backup). Every booking button on the site links to `/conversation.html#booking-embed`.

| Section id | Label | Purpose |
| --- | --- | --- |
| `conversation-hero` | | Eyebrow "30-minute conversation", headline, two-sentence intro (no button) |
| `book` (marker `conversation-book`) | | The calendar, directly under the intro: one line, inline Calendly iframe (`.booking-embed` with id `booking-embed`, loads straight away, 700px tall, 1000px under 640px wide), then the "Calendar not loading?" fallback and the email line. On a phone the calendar starts within the first screen |
| `conversation-talk` | 01 | What we'll talk about (four rows) |
| `conversation-details` | | Two rows: what you'll leave with (`conversation-leave-with`), before we meet (`conversation-before`) |

### Privacy: `privacy.html`

Draft notice written for this site; Ken to review (not legal advice). Its words live in the page itself.

| Section id | Purpose |
| --- | --- |
| `privacy-hero` | Heading, summary, "Last updated" date placeholder |
| `privacy-notice` | The full notice. Sub-headings have ids: `privacy-collects`, `privacy-why`, `privacy-where`, `privacy-retention`, `privacy-clients`, `privacy-choices`, `privacy-access` |
| `cta-band` | Shared CTA (heading and button only) |

### Not found: `404.html`

| Section id | Purpose |
| --- | --- |
| `notfound-hero` | "Sorry, that page isn't here." and a link home |
| `cta-band` | Shared CTA (heading and button only) |

GitHub Pages serves this for missing URLs. It uses root paths (`/assets/...`) so it works at any depth. It is `noindex`.

## For search engines and AI agents

The site is built to be read by AI agents as well as people: static HTML with no content hidden behind scripts, one `<h1>` per page, labelled sections, and these machine-readable files.

| File | Purpose | Keep in step |
| --- | --- | --- |
| `robots.txt` | Allows all crawlers and AI agents; points to the sitemap and `llms.txt` | Only if Ken wants to block a crawler |
| `sitemap.xml` | Every public page (not 404) | Add or remove a `<url>` when a page is added or removed; update `lastmod` |
| `llms.txt` | Plain-language summary for AI tools: audience, offers, prices, terms, principles, Ken's public facts, contact. Third person, no colons in prose, copy-editor has final say | Whenever an offer, price, term or public fact changes (it is public copy; facts must match `CLAUDE.md`) |
| `shared:machine` block in each page | Open Graph and schema.org data | Regenerated by `machine_data.py` (see Shared parts) |

The Calendly booking widget is an iframe that agents may not be able to use; the direct Calendly link and hello@draftera.ca are always shown beside it.

## Components (classes in site.css)

- `sec` + `split`: a section with a 1px Ink rule, green number label, heading on the left (4/12) and text on the right (8/12). `sec--no-rule` hides the rule.
- `flow`: steps on 2px rules (four by default; `pt-flow--three` or `flow--three` for three). `ladder` / `rung` (`free`, `feature`, `paid`): the offer staircase; `ladder--cards` makes equal-height cards with prices aligned at the bottom (four across, 2x2 under 1000px, one column under 600px; the `feature` card is Green with a Mint number). `weeks`: giant numerals. `checks`: numbered rows. `gets`: heading + text rows.
- `block`: Stone colour block. `proof`: big numbers on 2px rules. `example`: Mist panel. `panel` (`panel-price`, `panel-raised`, `panel-offer`): price and offer panels. `price-card`: hero aside.
- `fw`: the Draftera Foundations diagram (site.css). `fw-govern` (Stone, 2x2 cards) and `fw-use` (Mist, stacked) side by side 2:1, `fw-hub` Green bar beneath; stacks under 900px. Built by the same markup on Home (wrapped in one link, `fw-link`) and Foundations (each card links to `#piece-…`). Has a visually hidden text alternative (`figcaption`).
- `btn btn-primary` (one primary action per view), `btn-secondary`, `btn-link`.
- `portrait portrait-mini` (Home) and `portrait portrait-about` (About).

## Open to-dos

1. (Done 2026-09-29) **Booking link.** Calendly button and inline embed in `conversation.html` `#book`; privacy notice names Calendly.
2. (Done 2026-09-29) **Ken's photo** is on Home and About.
3. (Decided 2026-09-30: not now) **Self-host General Sans.** Kept on Fontshare because the repo is public (see "Font" above). Revisit only if the site moves to a private repo or other hosting.
4. **Privacy details.** Brevo (sending) and Gmail (inbox) added 2026-10-02. Ken still to confirm retention periods. Fontshare is listed in the notice and stays.
5. (Done 2026-09-30) **Wordmark outlines and PNG icons.**
6. **Workflow Build example.** `workflow-example` is illustrative; replace with a real case study once a client approves one (a `[TODO` comment marks the spot).
7. **Open copy questions** from earlier drafts are in `brand/archive/copy-2026-09-30/` under "Needs Ken".

## Example edit requests

- "Change the Workflow Build price to from $3,000." Update `CLAUDE.md`, then the `workflow-hero` price card, `workflow-price`, the `workflow-build.html` meta description, the `workflow-build` row on Services; then the offers in `machine_data.py` and `llms.txt`, and rerun `machine_data.py`.
- "Make the Home headline shorter." `index.html`, `home-hero` h1 (keep the `<em>` highlight), then regenerate `brand/copy/`.
- "Add an FAQ to the Foundations page." New section `framework-faq` before `cta-band`, numbered 08, using the FAQ style from `pages/workflow-build.css`.
- "Use a lighter green." Designer changes `--color-green` (and checks contrast) in `brand/tokens.css`; copy the change into `assets/css/tokens.css`.
- "Change the CTA band text on About." `about.html`, `section:cta-band`, the paragraph only.
- "Add a LinkedIn link to the footer." `section:site-footer` on all 10 pages.
