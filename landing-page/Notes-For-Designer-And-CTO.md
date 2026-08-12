# Handoff — designer & CTO

## For design

### Source of truth
- **Product light mode** is the visual source of truth (mint canvas, white cards, teal accents, black primary buttons).
- Company cards = **metric panels with score rings**, not collectible foil frames.
- Logo = teal dual-curve S mark (`site/img/stratemark-mark.svg`) + “Stratemark” wordmark in Parkinsans.

### Please review
1. Hero carousel card density vs product deck grid (padding, type sizes, ring, metric grid).
2. Mobile: carousel snap, CTA stack, tier grid.
3. Trust / How imagery: replace with final full-screen product captures when available (`docs/product-media/` has interim clean frames).
4. Tier reveal interaction (click tier → sentence under strip).
5. Pricing cards + heartfelt note tone.

### Assets to upgrade
- `site/img/logos/*.png` — favicon-grade; prefer official SVGs if license allows.
- Optional motion: short muted loops for How steps (describe / log / expand).

---

## For CTO

### Branch policy
- Work lives on **`landing-page` only**.
- **Do not merge to `main`** or attach production DNS until P0 in `STATUS.md` is green.
- Production domain deploy is **explicitly out of scope** for this handoff.

### Repo layout choice
Marketing site is under `landing-page/site/` so it does not collide with `apps/web`. If you prefer `apps/marketing` or root `public/`, relocate in a follow-up PR — keep `STATUS.md` with the move.

### Placeholders to search
```bash
rg "REPLACE-ME" landing-page/
rg "support@|Square Peg NG|\\[support" landing-page/
```

### Paddle readiness (summary)
Website needs obvious footer links to Privacy, Terms, Refunds, Contact (email **and** phone), clear product/pricing, SSL domain, MoR sentence in Terms, clean 30-day refund. Details: `docs/tobi-paddle-checklist.html` and Paddle seller handbook.

### Local verify
```bash
cd landing-page/site && python3 -m http.server 5174
```

### If this branch was delivered as a zip
Create the remote branch from `main` and push:

```bash
git fetch origin
git checkout main
git pull
git checkout -b landing-page
# unzip / copy landing-page/ folder to repo root
git add landing-page
git commit -m "feat(landing-page): marketing site handoff for design and CTO review"
git push -u origin landing-page
```

---

## Contact for questions on this handoff

Work was produced in Hyperagent against the product staging build and public fact-check sources (Aug 2026). Thread context retained by the requester; this folder is the durable artifact for the team.
