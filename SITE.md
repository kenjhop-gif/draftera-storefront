# draftera.ca site map

How the site is built and where everything lives, so any change can be made from a plain request. Read this before editing.

## Branches

| Branch | Use |
| --- | --- |
| `nonprofit-redesign` | Working branch. All edits happen here. |
| `(root)` | Live branch. GitHub Pages publishes draftera.ca from it. Never commit or push here without Ken's explicit yes (use the `publish-site` skill). Quote it in Git: `git checkout "(root)"`. |

Do not touch `CNAME` (it holds `draftera.ca`), GitHub Pages settings or DNS.

## Preview

From `website/`:

```
python -m http.server 8000
```

Then open http://localhost:8000/. Links and assets use root paths (`/assets/...`), so preview from `website/` itself, not a parent folder. Check pages at 375px and 1440px wide: no sideways scrolling.

## How the site is built

- Plain HTML and CSS. No build step and no JavaScript apart from the Google Analytics snippet.
- `_config.yml` only tells GitHub Pages not to publish this file (`SITE.md`). Add any other internal notes to its `exclude` list.
- `assets/css/tokens.css`: design tokens (colours, type, spacing, radii, rules), copied from `brand/tokens.css`. The bottom block, "Site additions", holds a few values the brand file doesn't name yet.
- `assets/css/site.css`: all layout and components. It uses only `var(--...)` tokens for colours. Never hard-code a colour in it; add or change a token instead.
- Font: General Sans, loaded from the Fontshare CDN (see to-dos).
- Logo: `assets/img/wordmark.svg` (light backgrounds) and `assets/img/wordmark-reversed.svg` (Green or Ink backgrounds), copied from `brand/logo/`. In the header and footer the wordmark is inlined as `<svg class="wordmark">` so it renders in General Sans; its colours come from CSS (`.wm-draft`, `.wm-era`).
- Favicon: `assets/img/favicon.svg` (a Paper "D" on Green, drawn as a path).
- Design reference: `brand/design-system.md` and `brand/directions/final/` (`index-portrait.html`, `about.html` and the offer-page direction).
- Copy source: `brand/website-copy.md`. Change the words there first (brand-writer), then here.

## Section markers

Every section on every page is wrapped in comment markers, and the `<section>` element carries the same id:

```html
<!-- section:home-hero -->
<section class="hero" id="home-hero">...</section>
<!-- /section:home-hero -->
```

To change a section, search for `section:<id>`. Section ids are stable; keep them when you edit. A new section gets a new marker pair and id in the pattern `<page>-<name>`.

Numbered sections show a small green number (`<p class="label">01</p>`). If you add, remove or reorder sections, renumber the labels on that page.

## Shared parts (repeated on every page: update all 7 pages together)

| Part | Marker | Notes |
| --- | --- | --- |
| Fonts link | `<!-- shared:fonts -->` in `<head>` | Fontshare stylesheet for General Sans |
| Analytics | `<!-- shared:analytics -->` in `<head>` | Google Analytics GA4 `G-L787TEY7BD`, Google signals and ad personalization off. Must stay on every page. |
| Header | `section:site-header` | Wordmark, nav (AI Foundations, Embedded AI Partnership, About Ken), "Book a conversation" button (to `/conversation.html`). Only `aria-current="page"` differs per page. Under 900px the nav drops to a second row. |
| CTA band | `section:cta-band` | Green block, heading "Book a 30-minute conversation with Ken", one paragraph that differs per page, button to `/conversation.html#booking-embed` (lands straight on the Calendly calendar; every booking button on the site, including the header button, uses this link), then the email backup line. On `conversation.html` the band has id `book` and its button is the booking action (see to-do 1). On `index.html` it also contains `<span id="contact">` so old `#contact` links still land. |
| Email backup line | `<p class="email-alt">` | "Prefer email? Write to hello@draftera.ca." (mailto link) directly under every "Book a 30-minute conversation with Ken" button: hero CTAs and every CTA band. Small secondary text; never a second button. Not in the header. |
| Footer | `section:site-footer` | Wordmark, tagline, links to all pages including Privacy ("Book a conversation"), location, hello@draftera.ca, copyright. Only `aria-current` differs. |

Each page also has its own `<title>`, meta description and canonical link in `<head>`, from `brand/website-copy.md`.

## Pages and sections

Exact wording lives in the HTML. `brand/copy/<page>.md` is generated from it by `.claude/skills/edit-site/export_copy.py`; run it after every change. Facts and offers must match `CLAUDE.md`. Updated 2026-09-30.

### Home: `index.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `home-hero` | | Eyebrow, headline (h1, "your team" in Green), lede, booking button, email line, Foundations link, small portrait with founder line and location |
| `home-situation` | 01 | Staff already use AI without a plan; Imagine Canada statistic as a callout |
| `home-help` | 02 | The work we help with: five task examples, closing line |
| `home-changes` | 03 | What you can expect, plus four flow steps |
| `home-ladder` | 04 | Offer staircase, three rungs (`ladder--three`): Introductory Conversation, Foundations Engagement (with founding-client price line), Embedded AI Partnership. Note on CAD plus taxes. AI Office Hours removed until it has a date |
| `home-operations` | 05 | Stone block: "Have an operational challenge that goes beyond AI?", four specific examples (`ops-gets`); not a separate package, fits the Partnership or a fixed-fee project |
| `home-principles` | 06 | Responsible, practical, measured |
| `home-lead` | | One line on a Stone strip: who leads the work, link to About |
| `cta-band` | | Shared CTA |

### Draftera AI Foundations Engagement: `foundations.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `foundations-hero` | | Headline, lede, booking button, price card (regular price with founding-client price under each, `pc-founding`), delivered-by line |
| `foundations-problem` | 01 | The work that crowds out the mission |
| `foundations-who` | 02 | Who it is for, numbered rows |
| `foundations-gets` | 03 | What the fee includes |
| `foundations-how` | 04 | Weeks 1 to 3 |
| `foundations-workflows` | 05 | Typical workflows we build |
| `foundations-example` | 06 | Illustrative example (replace with a real case study later; `[TODO` comment marks it) |
| `foundations-outcomes` | 07 | What you will have at the end |
| `foundations-data` | 08 | Data protection statement |
| `foundations-price` | 09 | One Stone panel: fee table (`fee-table`, regular vs founding-client price), then conditions (`fee-conditions`, id `foundations-founding-offer`): first two clients, ends December 31, 2026 or when filled, approved case study, CAD plus taxes, licences paid by client, 30-day credit |
| `foundations-faq` | 10 | Common questions, ending with "Can we do another project later without the Partnership?" (yes, fixed fee; Partnership preferred) |
| `cta-band` | | Shared CTA |

### Draftera Embedded AI Partnership: `partnership.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `partnership-hero` | | "An ongoing partner for AI and operations", lede, booking button, price card (Core, Plus, three-month minimum, scope lines) |
| `partnership-why` | 01 | Why ongoing support matters; callout names implementation, adoption, maintenance, operational judgment |
| `partnership-does` | 02 | Five monthly responsibilities: implementation, adoption, maintenance, operational judgment, reporting |
| `partnership-levels` | 03 | Core vs Plus table (`compare`, column ids `partnership-core`, `partnership-plus`): fee, scope, typical time as a guide, minimum term, suited to, review, quarterly report; credit note |
| `partnership-rhythm` | 04 | Monthly rhythm: review, build, train, report |
| `partnership-first-quarter` | 05 | Illustrative first three months (`pt-flow--three`) |
| `partnership-operations` | 06 | Stone block: operational challenges beyond AI, same four examples as Home; can be a monthly priority or a fixed-fee project |
| `partnership-accountable` | 07 | Accountable for results; what the quarterly board report covers |
| `partnership-terms` | 08 | Minimum term, after that, Foundations credit, prices and taxes |
| `partnership-fit` | 09 | Who the Partnership is for |
| `partnership-faq` | 10 | Common questions |
| `partnership-who` | | One line: who delivers the work, link to About |
| `cta-band` | | Shared CTA |

### About Ken: `about.html`

| Section id | Label | Purpose |
| --- | --- | --- |
| `about-hero` | | Headline "Over two decades of leading organizations...", lede, 4:5 portrait |
| `about-sla` | 01 | School Lunch Association, six years; proof numbers (3x revenue, 34 locations, 1 million+ meals, 80+ people) |
| `about-university` | 02 | "Leading operations in a large institution": operational leadership in a large Canadian university. Never say Ken works there now, never name the title or Memorial |
| `about-why` | 03 | Why AI, and why nonprofits and social enterprises; hands-on AI work with privacy and risk built in |
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
| `conversation-talk` | 02 | What we'll talk about |
| `conversation-details` | | Rows: leave with (`conversation-leave-with`), what to expect (`conversation-what-to-expect`), who it's for (`conversation-who`), before we meet (`conversation-before`) |
| `book` (marker `cta-band`) | | Green band: paragraph, inline Calendly iframe (`.booking-embed`, 700px tall, 1000px under 640px wide), then "Calendar not loading?" fallback link to Calendly and the email line |

### Privacy: `privacy.html`

Draft notice written for this site; Ken to review (not legal advice). Its words live in the page itself, not in `brand/website-copy.md`.

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

## Components (classes in site.css)

- `sec` + `split`: a section with a 1px Ink rule, green number label, heading on the left (4/12) and text on the right (8/12). `sec--no-rule` hides the rule.
- `flow`: steps on 2px rules (four by default; `pt-flow--three` for three). `ladder` / `rung` (`free`, `feature`, `paid`): the offer staircase. `weeks`: giant numerals. `checks`: numbered rows. `gets`: heading + text rows.
- `block`: Stone colour block. `proof`: big numbers on 2px rules. `example`: Mist panel. `panel` (`panel-price`, `panel-raised`, `panel-offer`): price and offer panels. `price-card`: hero aside.
- `btn btn-primary` (one primary action per view), `btn-secondary`, `btn-link`.
- `portrait portrait-mini` (Home) and `portrait portrait-about` (About).

## Open to-dos

1. (Done 2026-09-29) **Booking link.** Calendly button and inline embed in `conversation.html` `#book`; privacy notice names Calendly.
2. (Done 2026-09-29) **Ken's photo** is on Home and About.
3. **Self-host General Sans.** Follow "Self-hosting General Sans" in `brand/design-system.md`: put the WOFF2 files in `website/fonts/`, move the commented `@font-face` block in `assets/css/tokens.css` into use, remove the `shared:fonts` Fontshare links from all 7 pages, and preload the 600 weight. This also removes a third-party request (see privacy item 4).
4. **Privacy details.** Ken to set the "Last updated" date, name the email provider, confirm retention periods, and while fonts come from Fontshare, either self-host them or list Fontshare in the notice.
5. **Wordmark outlines.** Once fonts are self-hosted, outline the wordmark SVGs (designer) and add a PNG favicon / Apple touch icon for older browsers.
6. **Foundations example.** `foundations-example` is illustrative; replace with a real case study once a founding client approves one (a `[TODO` comment marks the spot).
7. **Open copy questions** from earlier drafts are in `brand/archive/copy-2026-09-30/` under "Needs Ken".

## Example edit requests

- "Change the Foundations Engagement price to $3,000 for one workflow." Update `brand/website-copy.md` and `CLAUDE.md`, then `foundations-hero` price card, `foundations-price`, the `foundations.html` meta description, and step 04 in `home-ladder`.
- "Make the Home headline shorter." `index.html`, `home-hero` h1 (keep the `<em>` highlight), plus `brand/website-copy.md`.
- "Add an FAQ to the AI Foundations page." New section `foundations-faq` before `cta-band`, numbered 07, using `checks` or `gets` rows.
- "Use a lighter green." Designer changes `--color-green` (and checks contrast) in `brand/tokens.css`; copy the change into `assets/css/tokens.css`.
- "Change the CTA band text on About." `about.html`, `section:cta-band`, the paragraph only.
- "Add a LinkedIn link to the footer." `section:site-footer` on all 7 pages.
- "Show Office Hours as open." `home-ladder`, step 03: remove the "Coming soon" line.
