# Design pass — what changed and why

**Date:** 12 August 2026
**Scope:** `02-Landing-Website/website/` — `index.html` and the four legal/contact pages
**Audience:** design + CTO
**Copy:** unchanged in substance. No claim, price, confidence label, or legal sentence was rewritten.
**Placeholders:** untouched. All six `REPLACE-ME` GitHub URLs and every yellow legal chip are still in place.

---

## 1. The headline change: one stylesheet instead of three

The page had accumulated three generations of CSS stacked on top of each other:

1. an original dark "collector's table" theme (cream cards on black felt, orange CTAs, a 3-card holo fan),
2. a `LIGHT APP MATCH OVERRIDES` block re-skinning it to the product's mint/teal light mode,
3. a `Feedback pass` block patching the hero carousel, tiers and pricing.

Layers 2 and 3 beat layer 1 with **89 `!important` declarations**, and layer 1 still shipped ~200 lines
of dead rules (`.tcard`, `.fan`, `.frame-foil`, `.frame-nv`, holo sweeps, float keyframes) that were parsed
and then hidden with `display:none !important`.

That is now **one authored stylesheet with zero `!important`**, organised
tokens → base → primitives → layout → components → responsive. Dead rules were deleted, not overridden.
This is P1 item #13 on your status doc, done.

**What this buys you:** a colour or spacing change is now a one-line token edit instead of an
archaeology exercise. Anything you change is the only rule that applies.

### Design tokens now in place
- **Type scale** — a 1.25 ratio replaces the hand-set values (8.2px, 10.5px, 13.5px, 14.5px, 15.5px, 17.5px…).
- **Spacing scale** — 4px base (`--s-1` … `--s-24`), plus one `--section-y` controlling vertical rhythm everywhere.
  The three inline `style="padding-top:40px"` overrides are gone.
- **Colour roles** — semantic (`--text`, `--text-muted`, `--brand`, `--border`) rather than the old
  aliasing where `--orange` had been redefined to teal.

---

## 2. Bugs found and fixed

| # | Bug | Detail |
|---|---|---|
| 1 | **Broken hero kicker** | Line 577 had lost its opening tag. "Competitive intelligence, card by card · Open source · BYOK" rendered as naked body text followed by a stray `</span>` — the pill styling never applied. Visible on the live page. |
| 2 | **Font declared but never loaded** | `h1,h2,h3` asked for `"Space Grotesk"`; the Google Fonts link loaded Parkinsans, Google Sans Flex, Inter and JetBrains Mono. Every heading was silently falling back. The link also had `&display=swap` twice. |
| 3 | **Legal links pointed off-site** | Every cross-link in the footer and in the legal pages pointed at `hyperagent.com/api/files/...` artifact URLs instead of the sibling `privacy.html` / `terms.html` / `refunds.html` / `contact.html` that ship in this folder. **46 links rewritten to relative paths.** Paddle requires these pages on your own domain — this would have failed review. |
| 4 | **Page scrolled sideways** | The hero carousel's scroll overflow leaked into the document: the page was 4177px wide in a 1440px window. It was masked by `body{overflow-x:hidden}`, so it looked fine while actually being broken. Fixed at the source (`contain:paint` on the rail, `minmax(0,1fr)` on the collapsed hero grid) and the mask removed, so a future leak will be visible instead of hidden. |
| 5 | **Contrast failures** | `--c-faint: #A3ACA9` on white is 2.33:1 and was used for CTA captions, card HQ lines, carousel hints and footer text. Card score labels and tier names used `#16A36A` at 9.5–12px (3.24:1). All text now clears WCAG AA — verified programmatically across 466 rendered text nodes, 0 failures. |
| 6 | **1MB HTML** | Product screenshots were inlined as base64. Extracted to `img/product/` and `img/logos2/`; **the HTML went from ~1,020KB to 92KB**. Every image now has `width`/`height` (no layout shift) and `loading="lazy"`. |
| 7 | **Same screenshot three times** | `deck.jpg` was used for How step 3, the Trust figure *and* the Second brain section. Steps 1/2/3 now use the three genuinely distinct captures you have (describe / research-log / deck) — P1 item #11, partially resolved (see §5). |

---

## 3. Section-by-section design changes

**Hero.** The headline clamp topped out at 84px and wrapped to four ragged lines at desktop; it now
maxes at 62px on a 13ch measure and sets in two. The gradient that faded into grey `#94A3B8` — making
"cards." the lowest-contrast text on the page — is now solid `--brand-deep`. The three stacked CTA
columns that overflowed their track are one row: primary, secondary, and a quiet text link, with the
captions consolidated into a single line beneath. Proof points sit below a rule as a real list.

**Carousel.** Real chevron icons replace the `‹` `›` glyphs. Arrows disable at each end, an edge fade
signals more content, and the rail takes keyboard input (←/→/Home/End) with a labelled group role.
Drag-to-scroll no longer swallows clicks. Cards gained internal rhythm: one 3×2 metric grid on a
consistent baseline instead of two separate grids with inline `font-size:15px` overrides.

**Trust.** The fake macOS window chrome (three grey dots) is gone — it framed a product screenshot as
if it were a browser mock. It's now a proper `<figure>` with a caption.

**How it works.** Consistent card structure with the media anchored at the bottom on a fixed 16:9 ratio,
so the three cards align regardless of source image dimensions.

**Tier strip.** Selection state moved from a `.is-on` class to `aria-pressed`, so the CSS and the
accessibility tree can't drift apart. The detail panel is `aria-live` and reverts on deselect.

**Second brain.** Previously a text column beside a third copy of `deck.jpg`. Now a 2×2 feature grid
with icons — no duplicate imagery, and it reads faster.

**Pricing.** This section explained itself four times: a bulleted lede, two cards, a `.price-note`, and
the `.paywall-heart`. Now: a one-line lede, two cards, and the pay-what-you-want note as a distinct
supporting block. The slider gained a $1–$100 scale so the range is legible before you drag it.
Same three facts, one pass instead of four.

**FAQ.** Chevron icon instead of a rotating `+`, tighter measure so the marker stays near its question.

**Footer.** Grew from a single link row into a three-column nav (Product / Legal / Source) — the legal
pages are now unmissable, which is what Paddle wants.

---

## 4. Accessibility and robustness

- **Skip link** and a real `<main>` landmark.
- **Focus states** — there were none; there is now one consistent `:focus-visible` ring.
- **Reveal animations** no longer depend on JavaScript to become visible. The old page set
  `opacity:0` on 40+ elements, animated them with GSAP from a CDN, and carried a `setTimeout(1600)`
  "last line of defense" to un-hide anything left invisible. The reveal is now opt-in via a `.js`
  class, so with JavaScript off or the CDN blocked, the page is simply a static readable page.
- **Two CDN dependencies removed** (GSAP + ScrollTrigger, ~70KB). Reveals, counters and the marquee
  are ~40 lines of vanilla JS.
- **`prefers-reduced-motion`** now genuinely disables the marquee and all transitions.
- Verified: one `<h1>`, no images missing `alt`, no unnamed links or buttons, sane tab order.

---

## 5. Still yours to do

Unchanged from your status doc — this pass deliberately did not touch these:

- **All P0 items.** Legal entity name, support email/phone, registered address, the six `REPLACE-ME`
  GitHub URLs, the Paddle checkout URL, domain/SSL, and the lawyer pass.
- **P1 #9 — higher-fidelity logos.** Still the 128px favicon-grade PNGs, now in `img/logos2/`.
  The card slot renders them at 26px so they hold up, but licensed SVGs would be better.
- **P1 #10 — native product recording.** Needs a capture we can't produce from here.
- **P1 #11 — How-section media.** The three steps now use three distinct stills, but step 3's deck view
  and the Trust figure are still the same capture. A dedicated "expand a card" frame would finish this.
- **Copy.** One suggestion, not actioned: the hero sub-paragraph and the `.hero-note` beneath the CTAs
  both explain the BYOK / one-time model, and the pricing section explains it twice more. There's room
  to say it once with more force — but that's a marketing call, not a design one.

---

## 6. File changes

```
02-Landing-Website/website/
  index.html            rewritten (1,220 → 1,163 lines; 1,020KB → 92KB)
  privacy.html          style block replaced with shared sheet; links localised
  terms.html            same
  refunds.html          same
  contact.html          same
  css/legal.css         NEW — shared by the four legal pages (was 4 copies of the same CSS)
  img/product/          NEW — describe.png, research-log.jpg, deck.jpg (extracted from base64)
  img/logos2/           NEW — 10 company logos (extracted from base64, named)
```

`img/` originals, `demo.html`, `firebase.json`, `DEPLOY.md` and `SWAP-POINTS.md` are untouched.
The original build is preserved in the thread if you want a side-by-side diff.

## 7. How to review

```bash
cd 02-Landing-Website/website && python3 -m http.server 5174
```

Then walk: hero → carousel (try the arrows and the keyboard) → tier strip → pricing slider → FAQ →
footer legal links. Resize to 390px and confirm nothing scrolls sideways.

---

## Addendum — 12 Aug, evening: app screenshots removed

At your request, **every product/app screenshot is gone from the site**.

Removed from `index.html`:
- the Trust section figure (deck view)
- all three How-it-works step images (describe / research log / deck)

Deleted from disk: `img/product/` and seven unreferenced legacy captures in `img/`
(`app-deck-clean.png`, `app-deck-light.png`, `deck-grid.jpg`, `dig.jpg`, `glassbox.jpg`,
`metrics.jpg`, `reader.jpg`).

**Kept:** the 10 company logos in the hero deck and the Stratemark brand mark — those are logos,
not app screenshots. Say the word if you want the company logos gone too.

Neither section was left with a hole:
- **Trust** — the three confidence cards (Verified / Estimated / Unknown) are now a full-width
  three-column band carrying the section, with the Brandeis quote beneath.
- **How it works** — text-led cards with a large ghost numeral where the screenshot used to sit,
  and a rule-and-label treatment on the step name.

Site weight is now **2.3MB total, 92KB of images** — all of it logos and the brand mark.
P1 item #10 (native product recording) is now the only thing standing between this page and
having real product imagery, so it moves up in priority if you want visuals back later.

---

## Addendum — 12 Aug, night: full visual rebuild

Brief: *"make it beautiful, add elements that make the site feel human, images are fine."*
The token architecture from the first pass is unchanged — this is a new visual register built on it.

### The idea

**The analyst's desk.** Paper-light canvas for the working sections, two ink-dark chapters for the
argument, one teal accent, warm sand for anything estimated. Rhythm: light → **dark** → light → **dark**.

### What's new

**Two dark chapters.** The Trust section and the final CTA now sit on near-black. "Glass box > black
box" is argued *in the dark*, next to a photograph of an actual glass cube — the metaphor made literal.

**Commissioned photography (2 images).**
- `img/art/glass-box.jpg` — a clear cube on a dark surface, internal structure visible in raking
  light, teal caustic pooling beneath. Anchors the Trust chapter.
- `img/art/desk.jpg` — an open notebook of handwritten figures with one entry circled and corrected,
  a fountain pen, cold coffee. Sits beside the "why pay what you want" note. The correction in the
  notebook is the whole product thesis in one image.

**A live research log** (`#log`) — the signature moment, and the answer to having no screenshots.
A looping, typed-out log of the product's actual loop: find a figure, name the source, or admit
there isn't one. It shows *verified*, *estimated* and **two "left blank"** lines, because the honest
gaps are the selling point. Renders fully static without JavaScript, and freezes complete under
`prefers-reduced-motion`.

**Human texture.** A paper-grain overlay at 3.5% on light, 5% on ink. A hand-drawn underline under
"deck of cards." A pinging live dot in the hero badge. Nav links that underline from the left on
hover. A blinking caret in the log. Cards that lift 3px.

### A deliberate omission

No stock founder photo, no invented testimonials, no fake customer logos. A product whose entire
claim is *"we will not invent a number"* cannot open with an invented person. The note is signed
"The Stratemark team" with the brand mark. Swap in a real portrait when you have one.

### Verified

| Check | Result |
|---|---|
| Contrast, light **and** dark sections | **511 text nodes, 0 failures** |
| `21st review index.html --strict` | **0 errors, 0 warnings, 0 suggestions** |
| Horizontal overflow @ 320/390/834/1280/1440 | none |
| Broken images | none |
| No-JS | full page readable, log renders all 11 lines statically |
| Reduced motion | marquee, log, reveals and transitions all disabled |
| Interactions | carousel, tier strip, slider, counters all pass |

Copy is still unchanged. Placeholders still intact. `demo.html` remains the only file with review
findings — it has never been part of these passes.

---

## Addendum — 12 Aug, late: ground-up redesign

Brief: *"completely redesign from the ground up, change the flow, keep only the logo and the text
content, build me a SaaS website, use Stripe / Google / Apple as inspiration."*

Everything below is new. Only the logo, the copy, the card data and the tier definitions carried over.

### The register

**Product-forward SaaS.** White canvas, near-black ink, one brand spectrum (teal → indigo).
Not the paper-editorial look of the previous passes — that direction is retired.

- **Type:** Inter Tight for display at -0.045em tracking, Inter for UI. Parkinsans and Google Sans
  Flex are gone. This is the typographic signature of the reference set (Stripe, Linear, Vercel).
- **Colour:** a single gradient ramp `#0F766E → #0D9488 → #4F46E5` used for the headline word,
  key numbers and the final CTA. A second, darker ramp (`--spectrum-deep`, teal → indigo, no light
  stop) is used behind small text and badges so every point clears 4.5:1 — the light teal is only
  ever behind display type, where 3:1 applies.
- **Geometry:** 24px and 32px radii, 1px hairline borders, low soft shadows.

### The new flow

| Was | Now |
|---|---|
| Split hero, copy left / deck right | **Centered hero** with aurora gradient bloom, then a full-width product panel below — Apple's centered-statement-then-artifact pattern |
| — | **Logo wall** — the 10 companies scored in the sample deck, greyscale, colour on hover |
| Trust chapter, How, Brain, Stats as separate bands | **One bento grid** — a large dark tile carrying the live research log, a confidence tile, three feature tiles, and a full-width BYOK strip |
| How = 3 cards | **Numbered flow** with a connecting hairline |
| Trust = dark section with photo | **Quote band** — a single 46px statement on ink, no imagery |
| Stats band | folded into the bento strip |
| FAQ single column | **Two-column FAQ** |
| Flat final CTA | **Gradient CTA card** — the page's visual climax |

Photography is gone. The proof is the product: real card UI, real logos, a running log.

### Verified

| Check | Result |
|---|---|
| Contrast (light + dark + gradient) | 477 nodes, 0 real failures; gradient ramps verified by interpolation (worst 5.46:1) |
| `21st review index.html --strict` | **0 errors, 0 warnings, 0 suggestions** |
| Overflow @ 320 / 390 / 834 / 1280 / 1440 | none |
| No-JS | full page readable, log renders all 10 lines |
| Reduced motion | aurora, log, reveals, transitions all stilled |
| Interactions | carousel, tier strip, slider, keyboard order all pass |

Copy unchanged. All `REPLACE-ME` placeholders and legal chips intact. Legal pages still on
`css/legal.css` — they were not part of this redesign and still carry the earlier styling.

---

## Addendum — 12 Aug, final: dark hero + real dithering

Brief: *"remove the pill entirely, hero background green gradient or dark, add dither effects,
make it stunning, it looks vibe coded."*

### The pill is gone

Removed outright — markup, styles, and the pulsing-dot keyframe. It was the most generic element
on the page. The hero now opens straight on the headline, which is a stronger move anyway.

*(Removing it exposed a bug I'd introduced: the cleanup regex that stripped `@keyframes ping`
stopped at the first `}` of a nested block and left an orphan `.on-ink ` selector, which silently
swallowed the `.tagline` rule — every section label had lost its brand colour. Found by diffing
computed styles against the stylesheet, fixed, and the whole sheet re-audited: braces balanced,
no undeclared custom properties, no orphan selectors.)*

### The hero is ink and emerald

Layered radial fields — deep emerald at the crown, a cold blue cast at the right — over near-black.
The headline is white; "deck of cards." carries a mint gradient that only works on dark.

### The dithering is real

Not a CSS approximation. Three assets generated from a recursively-built **8×8 Bayer matrix**,
thresholded per pixel:

| Asset | What it does |
|---|---|
| `img/tex/dither-bloom.png` | The hero's light source — an emerald bloom quantised through the matrix, so it stipples instead of blurring. 35KB. |
| `img/tex/dither-grain.png` | An 8×8 tile over every ink surface. Kills gradient banding and gives the black a printed tooth. 0.1KB. |
| `img/tex/dither-dissolve.png` | The hero doesn't end on a line — it **breaks apart into the white section below**, pixel by pixel. 1.4KB. |

The same bloom and grain now sit behind the trust quote band and the final CTA card, so the
dithering reads as a system rather than a one-off hero trick.

### Other craft

- **The product panel straddles the seam** — it starts on the dark hero and overhangs into the
  white below, which is what gives the fold depth.
- **The nav is transparent over the hero** (white text, no chrome) and swaps to a light, bordered
  bar once any light surface reaches it. The trigger watches *both* the dissolve edge and the
  panel's top edge, because the white panel arrives first — watching only the dissolve left white
  text on a white panel for ~300px of scroll.

### Verified

| Check | Result |
|---|---|
| `21st review index.html --strict` | **0 errors, 0 warnings, 0 suggestions** |
| Contrast | 476 nodes; only known false positives (white on gradient/transparent — both verified by hand at 5.46:1 worst case) |
| Overflow @ 320 → 1920 | none |
| Broken images | none |
| No-JS | full page readable, log renders all 10 lines |
| Reduced motion | dither assets are static; all animation stilled |

Stylesheet audit: braces balanced, 0 undeclared variables, 0 orphan selectors, still 0 `!important`.

---

## Addendum — typeface swap

Headings → **Parkinsans**. Body → **Google Sans Flex**. Inter Tight and Inter are gone from
`index.html`; the font request now loads Parkinsans, Google Sans Flex and JetBrains Mono only.

**Tracking was retuned, not just the family swapped.** Inter Tight is a condensed grotesk and was
set at -0.05em on the hero; Parkinsans is wider and rounder, and at that tracking the letterforms
collide. Every display value was opened up:

| Element | Was | Now |
|---|---|---|
| Global headings | -0.035em | -0.022em |
| Hero h1 | -0.05em / 0.98 lh | -0.032em / 1.02 lh |
| Body | -0.011em | -0.004em |
| Prices, stats, CTA h2 | -0.045em | -0.026em |
| Tier numerals | -0.04em | -0.022em |

Hero line-height also opened to 1.02 and the measure to 15ch, so the headline still sets in two
lines. Verified: fonts actually load (not a silent fallback), no clipped or overflowing text nodes,
no overflow 320 → 1920, contrast still 0 failures across 466 nodes, `21st review --strict` still clean.

Side effect worth noting: the legal pages already used Parkinsans + Google Sans Flex, so the
landing page and the legal pages are now typographically consistent for the first time.

---

## Addendum — humanising pass

Brief: *"the product section shouldn't be generic icon tiles; add real human photography,
community; bring life to it; improve the features layout."*

### Features: editorial, not icon tiles

The 2×3 icon-tile bento is gone. The product section is now an editorial split:

- **Left, sticky:** the section head plus a documentary photograph — a founder in a Lagos
  co-working space checking figures against her own handwritten notebook. That image *is* the
  product promise (research you can check), not decoration.
- **Right:** the five features as a numbered editorial list (01–05, JetBrains Mono counters,
  hairline separators) — no icons anywhere. The confidence states render as labelled chips
  with their dot colours inside feature 01 instead of a separate tile.
- **Below, full width:** the live research log in an ink panel with the three key stats
  ($0 / ~3 min / BYOK) beside its heading.

### New: "Built for people who check receipts"

A community band with two more documentary photographs — a team debating a figure at a laptop
("Settle the argument"), and an indie developer at his kitchen table at dusk ("Own your
research"). Captions sit on an ink gradient inside each photo; worst-case contrast computed at
17:1 over a mid-grey photo region.

All three photographs are commissioned documentary-style images: candid, nobody looking at
camera, no stock-photo posing, no visible screen content. People/photography direction:
founder (Lagos), team (evening office), developer (home). Total added weight ~440KB, all lazy-loaded
with dimensions.

### Verified

`21st review --strict` 0/0/0 · contrast 469 nodes 0 failures · overflow none 320→1920 ·
no-JS full render (log falls back to 10 static lines) · tier/carousel/slider interactions pass.

One deliberate rule kept: no invented testimonials, no fake customer logos, no named fake people.
The photographs show *kinds of users*, and the captions describe use cases — nothing pretends to
be a real customer quote.

### Correction — photography removed

Client call: the people photography read as generic. All three photos and the community section
are removed; `img/people/` deleted. The editorial numbered feature list stays. The three key stats
($0 / ~3 min / BYOK) moved into the sticky left column of the features section, and the log panel
head returned to a single column. The page's only imagery is now the product itself: the deck
carousel, the company logos, and the dithered ink surfaces.

---

## Addendum — 12 Aug, v2: full repositioning ("market intelligence, not decks")

Complete rebuild against a new product brief. The story is no longer "cards/decks" — the deck is
an output; the product is market intelligence. New promise: **Understand any market. Find what
matters.**

### Structure (5 sections + nav + footer)

1. **Hero** — eyebrow "AI Market Intelligence", editorial two-line headline, two CTAs, and the
   product itself as the centerpiece: a live-feeling workspace for the **AI Customer Support
   Market** (typed query, pulse strip, 5 company objects with momentum signals, 4 trend
   indicators with bars, one highlighted opportunity). Populates in stages on load.
2. **Problem** — "Market research is everywhere. / Understanding the market isn't." Scattered
   source chips flow into one market model.
3. **How it works** — three alternating steps: Research (query field), Understand (structure
   map: companies/products/categories/trends/customers/signals), Discover (opportunity insight).
4. **Ask the market** (dark) — a scripted conversation that plays on scroll; the market objects
   in a side panel light up in sync with each answer. Explicitly not a generic chatbot: answers
   carry signal/source citations.
5. **Act** — Research → Insights → Strategy → Presentation pipeline. The word "deck" appears
   exactly once, as an output: "Turn your research into a strategy deck when you're ready."

Plus a minimal final CTA ("Know the market. See what's next.") and a one-row footer.

### Removed relative to the previous site

Pricing section, FAQ, tier strip, logo wall, deck carousel, BYOK messaging, dithered dark hero.
The deck-led page is preserved at the repo as of commit 75c7319 and locally as
`prev-index-decks.html` in the thread workspace. Pricing/FAQ content still exists in the legal
pages and can return as a secondary page if needed.

### Design system

Warm paper (#FAF8F5), deep charcoal (#1A1A17), one teal accent (#0F766E). No gradients as
decoration, no glows, no glassmorphism. Parkinsans display + Google Sans Flex text +
JetBrains Mono for data. Different market objects get different treatments: company rows,
trend bars, highlighted opportunity insights, conversational AI, mono citations.

### Verified

21st review --strict 0/0/0 · contrast 174 nodes 0 failures · no horizontal overflow 320→1920 ·
no-JS fully readable (all content static) · reduced-motion: all animation stilled ·
animations verified in a real browser (typing, staged population, synced conversation).

### Note for marketing

The fact-checked Frontier AI figures and the verified/estimated/unknown vocabulary are not on
this page — the new brief centres the workflow, not the data honesty claim. If that trust story
should return, it fits naturally as a second row in the Problem section or a dedicated page.

---

## Addendum — 15 Aug: full redesign against the "explain the product" brief

Complete rebuild per the redesign prompt. Governing rule: **the repository is the source of truth.**
Every product visual on the page is reconstructed from `apps/web` — nothing is invented.

### Sourced from the codebase

| Marketing element | Source |
|---|---|
| Hero demo (prompt → phases → deck) | `features/deck/NewDeckPage.tsx` — phases, glow-border, input pill, region picker |
| Company card anatomy | `features/card/GameCard.tsx` — 44px logo tile, industry chip, 40px score ring, 2-line one-liner, HQ pin, metric grid, tier badge |
| Tier names T1–T8 | `packages/contracts/src/enums.ts` `TIER_LABELS` |
| Card types (Infrastructure, Distribution, Culture, Vice, Insight, Barrier to Entry) | `CARD_TYPE_LABELS` |
| Dashboard tabs (Overview, Metrics, Live Intel, Team & Org Chart, Products & Roadmap, Mission & Governance, History) | `DASHBOARD_TAB_LABELS` |
| Compare flow (select cards → "Compare these…") | `features/deck/DeckPage.tsx` compare mode |
| "Split into 8 tiers" market map | `DeckPage.tsx` Level-2 tier decks |
| Grounding line ("deck research + fresh Google Search — never model memory") | `features/deepdive/DeepDive.tsx` docstring |
| Verified / Estimated / Unverified badges | `features/factcheck/FactCheck.tsx` |
| Sample companies & metrics | `src/sample/frontier-snapshot.json` + the Aug-2026 fact-check |
| Palette | `src/index.css` light mode, verbatim |

### New copy (slogans out, product explanation in)

"Any market. As a deck of cards." → **"Research any market. Get the landscape in a deck."**
"Glass box > black box." → **"See the research behind the intelligence."**
"Maturity you can score." → **"Understand where companies stand."**
Plus: "Market research is scattered." / "Start with a question. End with a market map." /
"A deck is your market, organized." / "Stop switching between tabs." / "Keep researching the
same market." / "Chat gives you an answer. Stratemark gives you a market you can work with."

### Structure (15 sections)

Hero+demo · Problem (fragments→deck) · How it works · What is a deck (annotated card) ·
Company intelligence (dashboard) · Research transparency (dark) · Compare · Ask · Maturity ·
Market map (split into 8 tiers) · Use cases (4 product mini-scenarios) · Why vs general AI ·
Pricing (free-first, easy install secondary) · FAQ (10 questions) · Final CTA (dark).

Dark is reserved for hero, research engine and final CTA. Dither assets retained.

### Verified

21st review --strict 0/0/0 · contrast 447 nodes 0 failures · no overflow 320→1920 ·
no-JS fully readable · tier/slider/animations pass. Prior version preserved in git history.
