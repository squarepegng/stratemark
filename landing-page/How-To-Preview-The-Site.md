# Landing page

**Branch:** `landing-page`  
**Status:** Design / marketing handoff — **not production-deployed**  
**Last updated:** 11 August 2026  
**Owner thread:** Hyperagent landing rebuild (product marketing site)

> **Do not deploy this branch to the production domain until CTO + design review and placeholder swap-out are complete.**

This folder is a **self-contained static marketing site** for Stratemark. It is intentionally isolated from `apps/` so the product monorepo stays clean while marketing iterates.

---

## Quick start (local preview)

```bash
# from repo root
cd landing-page/site
python3 -m http.server 5174
# open http://localhost:5174
```

Or open `landing-page/site/index.html` directly in a browser (demo + relative links work best via a local server).

**Primary file:** `site/index.html`  
**Browser demo:** `site/demo.html` (linked from CTAs; may also be pointed at a hosted demo URL)

---

## Folder map

```
landing-page/
├── README.md                 ← this file
├── STATUS.md                 ← changes made + remaining work (read this first)
├── HANDOFF.md                ← designer + CTO checklist
├── site/                     ← static site root (Firebase hosting public dir)
│   ├── index.html            ← main landing page
│   ├── demo.html             ← embedded product demo (sample deck)
│   ├── privacy.html
│   ├── terms.html
│   ├── refunds.html
│   ├── contact.html
│   ├── firebase.json
│   ├── DEPLOY.md             ← Firebase hosting notes (do not deploy to prod domain yet)
│   ├── SWAP-POINTS.md        ← REPLACE-ME placeholders
│   └── img/                  ← logos, product stills, brand mark
└── docs/
    ├── factcheck/            ← Aug 2026 carousel metric sources
    ├── tobi-paddle-checklist.html
    └── product-media/        ← clean UI mocks + reference stills (no personal browser chrome)
```

---

## Design system (match the product app)

| Token | Value | Notes |
|---|---|---|
| Background | `#F4F8F7` | Light mint canvas (product light mode) |
| Surface | `#FFFFFF` | Cards, nav, panels |
| Primary teal | `#0F766E` | Accents, links, tier active |
| Bright teal | `#1CDCCD` | Logo gradient end |
| Ink / primary CTA | `#171A19` | Black buttons (not orange) |
| Positive | `#16A36A` | Verified / strong scores |
| Neutral / amber | `#D99A25` | Estimated confidence |
| Display font | **Parkinsans** | Headlines, card names, metric values |
| UI font | **Google Sans Flex** | Body, nav, controls |
| Logo | Teal S-curve mark | `site/img/stratemark-mark.svg` |

**Company cards** use the product’s **metric panel** pattern (logo tile, score ring, ARR / valuation / share, Tier · Highly Investable) — **not** foil trading-card frames.

---

## Related product code

- App monorepo: `apps/web`, `apps/desktop`, `packages/*`
- Existing one-file demo also lives at repo `demo/stratemark-demo.html` (legacy path; landing uses `landing-page/site/demo.html`)

---

## Contacts / placeholders

Legal entity working name: **Square Peg NG** (confirm exact CAC / Paddle spelling).  
See `docs/tobi-paddle-checklist.html` and `STATUS.md` § Remaining work.
