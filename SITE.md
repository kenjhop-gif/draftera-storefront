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
- Favicon: `assets/img/favicon.svg` (a Paper "D" on Green, drawn as a path), plus `favicon-32.png` (older browsers) and `apple-touch-icon.png` (180px, full-bleed Green; also used as the link-preview and schema.org image).
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
| Header | `section:site-header` | Wordmark, nav (Foundations, Services, Partnership, For partners, About Ken), "Book a conversation" button (to `/conversation.html`). Only `aria-current="page"` differs per page. Under 900px the nav drops to a second row. |
| CTA band | `section:cta-band` | Green block, heading "Book a 30-minute conversation with Ken", one paragraph that differs per page, button to `/conversation.html#booking-embed` (lands straight on the Calendly calendar; every booking button on the site, including the header button, uses this link), then the email backup line. On `conversation.html` the band has id `book` and its button is the booking action (see to-do 1). On `index.html` it also contains `<span id="contact">` so old `#contact` links still land. |
| Email backup line | `<p class="email-alt">` | "Prefer email? Write to hello@draftera.ca." (mailto link) directly under every "Book a 30-minute conversation with Ken" button: hero CTAs and every CTA band. Small secondary text; never a second button. Not in the header. |
| Footer | `section:site-footer` | Wordmark, tagline, links (Home, Draftera Foundations, Services, Workflow Build, Embedded AI Partnership, For partners, About Ken, Book a conversation, Privacy), location, hello@draftera.ca, copyright. Only `aria-current` differs. |

Each page also has its own `<title>`, meta description and canonical link in `<head>`.

## Pages and sections

Exact wording lives in the HTML. `brand/copy/<page>.md` is generated from it by `.claude/skills/edit-site/export_copy.py`; run it after every change. Facts and offers must match `CLAUDE.md`. Updated 2026-11-10 (foundations-rework: framework page, Services, Workflow Build, For partners; brief in `draftera-website-brief.md` at the project root).

### Home: `index.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `home-hero` | | Eyebrow, headline "Practical AI, built with your team." (h1, "your team" in Green), lede, two buttons (`hero-buttons`: booking, "See the services"; stacked above 1200px so the lede keeps its width), email line, small portrait with founder line |
| `home-situation` | 01 | Staff already use AI without shared guidelines; Imagine Canada statistic as a callout |
| `home-help` | 02 | The work I help with: five task examples, closing line |
| `home-changes` | 03 | What you can expect, plus four flow steps |
| `home-foundations` | 04 | Draftera Foundations: subtitle, paragraph, the framework diagram (`fw`, whole diagram links to `/foundations.html`), "See the framework" and "See the services" links |
| `home-ladder` | 05 | "How we work together" as four equal-height priced cards (`ladder ladder--cards`): Introductory Conversation (Free), Start with one piece (From $950), Build the whole (Full Foundations, From $6,500, the Green `feature` card), Keep it current (From $1,500 per year). CAD and taxes note |
| `home-operations` | 06 | Stone block: "Have an operational challenge that goes beyond AI?", four specific examples (`ops-gets`); not a separate package, fits the Partnership or a fixed-fee project |
| `home-sessions` | 07 | "Information sessions and workshops on AI": three areas each session covers (`hw-list`), "Sessions start from $950", link to Training on Services |
| `home-principles` | 08 | Responsible, practical, measured |
| `home-lead` | | One line on a Stone strip: who leads the work, link to About |
| `cta-band` | | Shared CTA |

### Draftera Foundations (the framework): `foundations.html`

Always pair the name with its subtitle on first use on a page: "a framework for using AI in nonprofits and social enterprises" (no "well", Ken 2026-11-10). Draftera prepares organizations for independent verification; it never certifies, accredits or approves. Never "AI Ready". The framework "lines up with" or "is being mapped to" national standards, never "compliant with".

| Section id | Label | Purpose |
| --- | --- | --- |
| `framework-hero` | | Eyebrow "The framework", h1 "Draftera Foundations", subtitle |
| `framework-purpose` | 01 | Why it exists; Imagine Canada figures (80% use AI, 10% have a policy, 64% of users have none and aren't developing one; checked against the report 2026-11-10) |
| `framework-who` | 02 | Who it is for |
| `framework-pieces` | 03 | The diagram (`fw`, each card links to its row) and the seven descriptions (`gets fw-pieces`, ids `piece-leadership`, `piece-people`, `piece-data`, `piece-oversight`, `piece-transparency`, `piece-value`, `piece-inventory`) |
| `framework-evidence` | 04 | International frameworks, Canadian standards and guidance, sector evidence |
| `framework-use` | 05 | Start with one piece, build the whole, seek independent verification if desired (`flow flow--three`) |
| `framework-principles` | 06 | People stay responsible, plain language, documented evidence, independence |
| `framework-scope` | 07 | Not legal advice; "Version 0.1, November 2026. Reviewed annually."; link to Services |
| `cta-band` | | Shared CTA (heading and button) |

### Services: `services.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `services-hero` | | Eyebrow "Services and prices", h1, subtitle, intro naming the framework |
| `services-one` | 01 | Start with one piece (`gets svc-list`; each row: name, from-price, piece label linking to the framework). Row ids `ai-policy` (from $1,500), `training` (from $950 per session; "Information sessions and workshops…"), `data-privacy-review` (from $1,250), `workflow-build` (from $2,500; two from $4,000; final report, no time-saved promise), `transparency-statement` (from $350 add-on) |
| `services-whole` | 02 | `full-foundations` (from $6,500), `foundations-check-up` (from $1,500 per year) |
| `services-ongoing` | 03 | The Partnership, link |
| `services-networks` | 04 | One session for several organizations, link to For partners |
| `services-start` | 05 | Free conversation, written fixed-price proposal; fine print on CAD, taxes, size of organization, Digital Governance Council fee |
| `cta-band` | | Shared CTA |

### Workflow Build: `workflow-build.html`

The former Draftera AI Foundations Engagement page, moved and renamed 2026-11-10. Styles in `pages/workflow-build.css`.

| Section id | Label | Purpose |
| --- | --- | --- |
| `workflow-hero` | | Eyebrow "Workflow Build", headline, lede, framework line (value and measurement, and human oversight), booking button, price card (From $2,500 one workflow, From $4,000 two), delivered-by line |
| `workflow-problem` | 01 | The work that crowds out the mission |
| `workflow-who` | 02 | Who it is for, numbered rows |
| `workflow-gets` | 03 | What the fee includes |
| `workflow-how` | 04 | Weeks 1 to 3 |
| `workflow-workflows` | 05 | Typical workflows I build |
| `workflow-example` | 06 | Illustrative example (replace with a real case study later; `[TODO` comment marks it) |
| `workflow-outcomes` | 07 | What you will have at the end |
| `workflow-data` | 08 | Data protection statement |
| `workflow-price` | 09 | Stone panel: fee table (from-prices only) and one conditions paragraph (CAD, taxes, size of organization, licences) |
| `workflow-faq` | 10 | Common questions, ending with "Can we do another project later without the Partnership?" |
| `cta-band` | | Shared CTA |

### For partners: `partners.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `partners-hero` | | Eyebrow "Partner work", h1 "For partners", subtitle, opening paragraph |
| `partners-need` | 01 | The need (Imagine Canada figures and barriers) |
| `partners-networks` | 02 | Networks and associations, three bullets (`partner-list`) |
| `partners-funders` | 03 | Funders, three bullets |
| `partners-sector` | 04 | Sector and standards bodies, three bullets |
| `partners-framework` | 05 | The framework behind the work, link |
| `partners-talk` | 06 | Let's talk: scoped to fit, email or book; no prices |
| `cta-band` | | Shared CTA |

All nine bullets confirmed by Ken 2026-11-10.


### Draftera Embedded AI Partnership: `partnership.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `partnership-hero` | | "An ongoing partner for AI and operations", lede, booking button, price card (Core, Plus, three-month minimum, scope lines) |
| `partnership-why` | 01 | Why ongoing support matters, ending "For organizations that have put the framework in place, the Partnership keeps it working day to day." (no Check-up promise); callout names implementation, adoption, maintenance, operational advice |
| `partnership-does` | 02 | Five areas of support: implementation, adoption, maintenance, operational advice, reporting |
| `partnership-levels` | 03 | Core vs Plus table (`compare`, column ids `partnership-core`, `partnership-plus`): fee, scope, typical time as a guide, minimum term, suited to, review, quarterly report; credit note |
| `partnership-rhythm` | 04 | Monthly rhythm: review, build, train, report |
| `partnership-first-quarter` | 05 | Illustrative first three months (`pt-flow--three`) |
| `partnership-operations` | 06 | Stone block: operational challenges beyond AI, same four examples as Home; can be a monthly priority or a fixed-fee project |
| `partnership-accountable` | 07 | Accountable for results; what the quarterly board report covers |
| `partnership-terms` | 08 | Minimum term, after that, prices and taxes (Foundations credit removed 2026-11-10) |
| `partnership-fit` | 09 | Who the Partnership is for |
| `partnership-faq` | 10 | Common questions |
| `partnership-who` | | One line: who delivers the work, link to About |
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

The free first step is the **Introductory Conversation** (30 minutes, Google Meet, booked through Calendly at https://calendly.com/hello-draftera/30min, email as backup). Every booking button on the site links to `/conversation.html#booking-embed`.

| Section id | Label | Purpose |
| --- | --- | --- |
| `conversation-hero` | | Headline, lede, button to `#booking-embed`, email line |
| `conversation-how` | 01 | How the call works |
| `conversation-talk` | 02 | What we'll talk about (no eyebrow) |
| `conversation-details` | | Rows: leave with (`conversation-leave-with`), what to expect (`conversation-what-to-expect`), who it's for (`conversation-who`), before we meet (`conversation-before`) |
| `book` (marker `cta-band`) | | Green band: paragraph, inline Calendly iframe (`.booking-embed`, 700px tall, 1000px under 640px wide), then "Calendar not loading?" fallback link to Calendly and the email line |

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
