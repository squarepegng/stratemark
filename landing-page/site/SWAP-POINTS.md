# Swap points before production deploy

Search the site for `REPLACE-ME` and yellow placeholder chips on legal pages.

## Required swaps

1. **GitHub repository URL — DONE (12 Aug)**  
   All links now point at `https://github.com/squarepegng/stratemark` (nav, hero, pricing, footer).

2. **Demo URL** (optional)  
   Hero “Try the demo” may point at a hosted demo. Default ships with `demo.html` in this folder and/or a Hyperagent-hosted demo URL embedded during build — confirm which URL should ship.

3. **Paddle checkout URL**  
   Easy-install primary button currently may point at GitHub releases. Swap to Paddle checkout when live.

4. **Square Peg NG legal details** (privacy / terms / refunds / contact)  
   - Exact legal name (match Paddle + CAC)  
   - Support email  
   - Support phone  
   - Registered address  
   - RC / tax IDs if any  

5. **og:image** (nice-to-have)  
   Add a 1200×630 share card. Favicon is now wired to `img/stratemark-mark.svg`.

## Do not

- Deploy to the production custom domain until CTO signs off on this list.

## Resolved in the 12 Aug design pass

- Footer and legal cross-links now point at the local `privacy.html` / `terms.html` /
  `refunds.html` / `contact.html` (they previously pointed at external Hyperagent artifact
  URLs, which would not have satisfied Paddle's "legal pages on your domain" requirement).
- Favicon wired to the brand mark.

See `01-READ-ME-FIRST/Design-Pass-Changelog.md`.
