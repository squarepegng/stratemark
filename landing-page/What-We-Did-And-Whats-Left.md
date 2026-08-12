# Landing page — status report

**Branch:** `landing-page`  
**Date:** 11 August 2026  
**Audience:** CTO, design, marketing  
**Intent:** Handoff of marketing site work completed in Hyperagent. **Main is untouched.**

---

## 1. Executive summary

We rebuilt the Stratemark **marketing landing page** to match the **current product light-mode UI** (metric cards, teal brand, black CTAs, Parkinsans / Google Sans Flex). The site is static HTML/CSS/JS, ready for local preview and later Firebase (or other) hosting.

**Not done:** production domain deploy, real GitHub/Paddle URLs, Square Peg NG legal contact fill-in, and a full native app screen-recording of type → research → expand deck (clean stills + research-log demo are included instead).

---

## 2. What changed (completed)

### Brand & chrome
- [x] Light-mode product palette (`#F4F8F7` canvas, white surfaces, teal `#0F766E`, black CTAs)
- [x] Real Stratemark **S-mark logo** SVG + title-case **Stratemark** wordmark
- [x] Fonts: **Parkinsans** (display) + **Google Sans Flex** (UI)
- [x] Removed orange “deck stack” logo and orange gradient CTAs
- [x] Removed congested hero kicker pill

### Hero
- [x] Removed skinny top row of mini-cards
- [x] Single **large centered carousel** of company metric cards (product card anatomy)
- [x] 10 Frontier AI companies with favicon logos
- [x] Confidence labels on metrics: verified / estimated / unknown
- [x] Carousel controls: arrows, dots, drag-scroll
- [x] CTAs: Try the demo · Easy install · GitHub (BYOK messaging)

### Trust / How / Brain
- [x] Trust headline: **“Glass box > black box.”**
- [x] Industry quote: Brandeis (“Sunlight is said to be the best of disinfectants.”)
- [x] Cleaner product stills (cropped / mock frames without personal browser tabs)
- [x] How steps copy: type market → live research log → expand deck
- [x] Brain copy: ask anywhere, PDF export, compare cards
- [x] Maturity section: **“Maturity you can score.”** (no “rarity” / foil marketing language)
- [x] Interactive tiers T1–T8 with click-to-reveal plain-language breakdowns (product tier names + short definitions)

### Pricing & legal structure
- [x] Clear split: **open source** vs **one-time easy install** vs **bring your own key**
- [x] Pay-what-you-want $1–100 slider (one-time); fill tracks thumb
- [x] Heartfelt “why pay what you want” note for open-source sustainability
- [x] Removed “free forever” and “no subscription ever” claims
- [x] Separate legal pages (footer links only — not a legal wall on the homepage): Privacy, Terms, Refunds, Contact
- [x] Paddle MoR sentence in Terms (required handbook wording)
- [x] 30-day money-back refund draft
- [x] Working name **Square Peg NG** on legal drafts + yellow placeholders for missing contacts

### Metrics accuracy
- [x] Carousel figures **fact-checked against public sources (August 2026)** and labeled
- [x] Major corrections documented (e.g. Anthropic valuation $380B → **$965B** Series H; ARR run-rate **$47B**)
- [x] Source table: `docs/factcheck/frontier-ai-aug2026.md`

### FAQ
- [x] Expanded FAQ: demo contents, BYOK cost model, hallucination rules, deck composition (card types), eight tiers, data locality, open source vs easy install, future subscription disclosure

---

## 3. Remaining work (action required)

### P0 — Blockers before any public / paid launch

| # | Item | Owner | Notes |
|---|---|---|---|
| 1 | **Confirm exact legal entity name** | Tobi / Square Peg NG | Must match CAC **and** Paddle account character-for-character. Drafts use “Square Peg NG”. |
| 2 | **Support email + phone** | Tobi | Required on Contact + legal pages for Paddle. Prefer dedicated business / Google Voice over personal NG mobile. |
| 3 | **Registered Nigerian address** (+ RC / tax IDs if any) | Tobi | Contact + Privacy |
| 4 | ~~Swap `REPLACE-ME` GitHub URLs~~ **DONE 12 Aug** | CTO | All links now point at `github.com/squarepegng/stratemark` |
| 5 | **Paddle checkout URL** | CTO / Tobi | Easy-install CTA currently points at GitHub releases placeholder |
| 6 | **Production domain + SSL** | CTO | Do **not** point production domain at this branch until review. Domain verification required for Paddle. |
| 7 | **Lawyer pass on Privacy / Terms / Refunds** | Tobi counsel | Drafts only; 30-day refund is Paddle-preferred style |

See also: `docs/tobi-paddle-checklist.html`

### P1 — Design / product polish

| # | Item | Owner | Notes |
|---|---|---|---|
| 8 | **Design QA pass** on spacing, type scale, mobile carousel | Design | Hero carousel cards should match product `fge` card component as closely as possible |
| 9 | **Higher-fidelity logos** | Design | Current marks are favicon-quality PNGs under `site/img/logos/` — swap for brand SVGs where licensed |
| 10 | **Native product screen recording** | Design / Eng | Type market → research log → expand deck, **full screen, no Hyperagent chrome**. Clean stills + research-log demo are in `docs/product-media/`; a full SPA recording needs direct app URL or local `pnpm dev` capture |
| 11 | **How-section media variety** | Design | Prefer distinct stills/clips per step (describe / log / expand), not one repeated frame |
| 12 | **Demo multi-niche copy** | Marketing | When multi-market demo ships, update FAQ + hero CTA counts (“5 market decks” etc.) |
| 13 | **Remove dead CSS** | Eng optional | Legacy `.tcard` / `.frame-nv` rules may remain unused in `index.html` — safe to purge in a cleanup PR |

### P2 — Engineering integration

| # | Item | Owner | Notes |
|---|---|---|---|
| 14 | Decide hosting (Firebase per `site/DEPLOY.md` vs monorepo static host) | CTO | Keep **off production domain** until P0 complete |
| 15 | Wire landing CTAs to real app routes / download artifacts | CTO | Align with desktop release pipeline |
| 16 | Optional: rebuild landing into `apps/web` marketing route | CTO | Only if we want one deployable; current isolation is intentional |

---

## 4. Explicit non-goals (this branch)

- **No production domain deploy** from this workstream  
- **No merge to `main`** without CTO approval  
- **No Paddle go-live** until legal placeholders filled and domain approved  
- **Not a substitute for the product app** in `apps/` — marketing only  

---

## 5. How to review

1. Check out `landing-page`  
2. `cd landing-page/site && python3 -m http.server 5174`  
3. Walk: hero carousel → tier clicks → trust/how screenshots → pricing slider → FAQ → footer legal pages  
4. File design notes against `HANDOFF.md`  

---

## 6. Key files for CTO / design

| Need | Path |
|---|---|
| Landing HTML | `landing-page/site/index.html` |
| Brand mark | `landing-page/site/img/stratemark-mark.svg` |
| Metric fact-check | `landing-page/docs/factcheck/frontier-ai-aug2026.md` |
| Paddle / Square Peg checklist | `landing-page/docs/tobi-paddle-checklist.html` |
| Clean product media references | `landing-page/docs/product-media/` |
| Placeholder map | `landing-page/site/SWAP-POINTS.md` |
