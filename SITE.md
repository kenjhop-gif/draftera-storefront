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
| CTA band | `section:cta-band` | Green block, heading "Book a 30-minute conversation with Ken", one paragraph that differs per page, button to `/conversation.html`, then the email backup line. On `conversation.html` the band has id `book` and its button is the booking action (see to-do 1). On `index.html` it also contains `<span id="contact">` so old `#contact` links still land. |
| Email backup line | `<p class="email-alt">` | "Prefer email? Write to hello@draftera.ca." (mailto link) directly under every "Book a 30-minute conversation with Ken" button: hero CTAs and every CTA band. Small secondary text; never a second button. Not in the header. |
| Footer | `section:site-footer` | Wordmark, tagline, links to all pages including Privacy ("Book a conversation"), location, hello@draftera.ca, copyright. Only `aria-current` differs. |

Each page also has its own `<title>`, meta description and canonical link in `<head>`, from `brand/website-copy.md`.

## Pages and sections

Copy locations refer to headings in `brand/website-copy.md`.

### Home: `index.html`

| Section id | Purpose | Copy in website-copy.md |
| --- | --- | --- |
| `home-hero` | Eyebrow, headline (h1, "your team" in Green), subhead, CTA button, Foundations Engagement link, small portrait with founder line | Home > Headline, Subhead, Founder line |
| `home-situation` | 01: staff already use AI; Imagine Canada statistic as a callout | Home > "Your staff are already using AI..." |
| `home-changes` | 02: what changes, plus four flow steps | Home > "What changes when AI is part of how you work" (the four step labels condense that text) |
| `home-ladder` | 03: offer staircase, five steps; Foundations Engagement step in solid Green | Home > "How we can work together" |
| `home-foundations` | 04: the three Foundations Engagement weeks with giant numerals, link to the AI Foundations page | AI Foundations > How it works (headline from AI Foundations > Headline) |
| `home-story` | 05: Stone block, "Why I do this", pull quote, proof numbers | Home > "Why I do this" (numbers are from Ken's proof points in `CLAUDE.md`) |
| `cta-band` | Shared CTA | Home > "Book a 30-minute conversation with Ken" |

### Draftera AI Foundations Engagement: `foundations.html`

| Section id | Purpose | Copy |
| --- | --- | --- |
| `foundations-hero` | Eyebrow, headline, subhead (lede) plus the fixed-fee paragraph, CTA, price card aside ($2,500 / $4,000) | AI Foundations > Headline, Subhead, Price |
| `foundations-who` | 01: who it is for, numbered rows | AI Foundations > Who it is for |
| `foundations-gets` | 02: what you get, heading + text rows | AI Foundations > What you get |
| `foundations-how` | 03: weeks 1–3 | AI Foundations > How it works |
| `foundations-example` | 04: illustrative example in a Mist panel | AI Foundations > An example... |
| `foundations-data` | 05: data protection statement | AI Foundations > Your data stays protected |
| `foundations-price` | 06: price panel (Stone) and founding client offer panel (Ink, id `foundations-founding-offer`) | AI Foundations > Price, Founding client offer |
| `cta-band` | Shared CTA | AI Foundations > Book a 30-minute conversation with Ken |

### Draftera Embedded AI Partnership: `partnership.html`

| Section id | Purpose | Copy |
| --- | --- | --- |
| `partnership-hero` | Headline, subhead, CTA, price card aside (Core, Plus) | Embedded AI Partnership > Headline, Subhead, Two levels |
| `partnership-why` | 01: why ongoing support | Embedded AI Partnership > Why ongoing support |
| `partnership-does` | 02: what I do, numbered rows | Embedded AI Partnership > What I do as your embedded AI partner |
| `partnership-levels` | 03: Core (id `partnership-core`) and Plus (id `partnership-plus`) panels, engagement fee credit note | Embedded AI Partnership > Two levels |
| `partnership-accountable` | 04: accountable for results | Embedded AI Partnership > Accountable for results |
| `cta-band` | Shared CTA | Embedded AI Partnership > Book a 30-minute conversation with Ken |

### About Ken: `about.html`

| Section id | Purpose | Copy |
| --- | --- | --- |
| `about-hero` | Headline, subhead, 4:5 portrait (placeholder) | About > Headline, Subhead |
| `about-sla` | 01: School Lunch Association, plus proof numbers | About > Running the School Lunch Association |
| `about-university` | 02: senior university administration (Memorial not named) | About > Senior university administration |
| `about-why` | 03: why AI, why nonprofits | About > Why AI, and why nonprofits |
| `about-credentials` | 04: credentials, numbered rows | About > Credentials |
| `about-where` | 05: where I work | About > Where I work |
| `cta-band` | Shared CTA | About > Book a 30-minute conversation with Ken |

### Book a conversation: `conversation.html`

The free first step is the **Introductory Conversation** (30 minutes, Google Meet, booked through Google Calendar appointment schedules, email as backup).

| Section id | Purpose | Copy |
| --- | --- | --- |
| `conversation-hero` | Eyebrow, headline, subhead, button that jumps to `#book`, email line | Book a conversation > Headline, Subhead |
| `conversation-how` | 01: how it works | Book a conversation > How it works |
| `conversation-talk` | 02: what we'll talk about, numbered rows | Book a conversation > What we'll talk about |
| `conversation-details` | Rows: leave with (`conversation-leave-with`), what it isn't (`conversation-what-it-isnt`), who it's for (`conversation-who`), before we meet (`conversation-before`) | Book a conversation > the matching headings |
| `book` (marker `cta-band`) | Green band. For now the button is `mailto:hello@draftera.ca?subject=Introductory%20conversation`, labelled "Email to book a time", then the email line. A `BOOKING LINK GOES HERE` comment holds the swap-in markup | Book a conversation > Book a 30-minute conversation with Ken |

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
- `flow`: four steps on 2px rules. `ladder` / `rung` (`free`, `feature`, `paid`): the offer staircase. `weeks`: giant numerals. `checks`: numbered rows. `gets`: heading + text rows.
- `block`: Stone colour block. `proof`: big numbers on 2px rules. `example`: Mist panel. `panel` (`panel-price`, `panel-raised`, `panel-offer`): price and offer panels. `price-card`: hero aside.
- `btn btn-primary` (one primary action per view), `btn-secondary`, `btn-link`.
- `portrait portrait-mini` (Home) and `portrait portrait-about` (About).

## Open to-dos

1. **Booking link.** Waiting for Ken's Google Calendar appointment schedule URL. In `conversation.html`, `#book`, follow the `BOOKING LINK GOES HERE` comment: add the "Pick a time..." paragraph, point the button at the booking page with the label "Book a 30-minute conversation with Ken", and optionally add the inline embed (`.booking-embed` iframe: full width, min-height 600px, has a `title`). Nothing else changes; the other pages already link to `/conversation.html`. Privacy already names Google Calendar and Google Meet.
2. **Ken's photo.** Save it as `assets/img/ken.jpg` (portrait, 4:5, at least 800 x 1000). Then in `index.html` (`home-hero`) and `about.html` (`about-hero`), replace the placeholder `<figure>` with the `<img>` line given in the comment right above it.
3. **Self-host General Sans.** Follow "Self-hosting General Sans" in `brand/design-system.md`: put the WOFF2 files in `website/fonts/`, move the commented `@font-face` block in `assets/css/tokens.css` into use, remove the `shared:fonts` Fontshare links from all 7 pages, and preload the 600 weight. This also removes a third-party request (see privacy item 4).
4. **Privacy details.** Ken to set the "Last updated" date, name the email provider, confirm retention periods, and while fonts come from Fontshare, either self-host them or list Fontshare in the notice.
5. **Wordmark outlines.** Once fonts are self-hosted, outline the wordmark SVGs (designer) and add a PNG favicon / Apple touch icon for older browsers.
6. **Foundations example.** `foundations-example` is illustrative; replace with a real case study once a founding client approves one (a `[TODO` comment marks the spot).
7. **Open copy questions** are listed under "Needs Ken" in `brand/website-copy.md`.

## Example edit requests

- "Change the Foundations Engagement price to $3,000 for one workflow." Update `brand/website-copy.md` and `CLAUDE.md`, then `foundations-hero` price card, `foundations-price`, the `foundations.html` meta description, and step 04 in `home-ladder`.
- "Make the Home headline shorter." `index.html`, `home-hero` h1 (keep the `<em>` highlight), plus `brand/website-copy.md`.
- "Add an FAQ to the AI Foundations page." New section `foundations-faq` before `cta-band`, numbered 07, using `checks` or `gets` rows.
- "Use a lighter green." Designer changes `--color-green` (and checks contrast) in `brand/tokens.css`; copy the change into `assets/css/tokens.css`.
- "Change the CTA band text on About." `about.html`, `section:cta-band`, the paragraph only.
- "Add a LinkedIn link to the footer." `section:site-footer` on all 7 pages.
- "Show Office Hours as open." `home-ladder`, step 03: remove the "Coming soon" line.
