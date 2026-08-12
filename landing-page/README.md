# Stratemark marketing site — `landing-page` branch

Static marketing site. **Do not merge to `main` or point the production domain here until the
P0 items in `What-We-Did-And-Whats-Left.md` are green.**

## Layout
- `site/index.html` — the landing page. Binary images (company logos, dither textures) are
  inlined as data URIs because this branch was committed through the GitHub API, which only
  carries text. Extract them back to `site/img/` in a follow-up commit if preferred.
- `site/privacy|terms|refunds|contact.html` + `site/css/legal.css` — legal pages (drafts;
  yellow chips mark fields awaiting Square Peg NG details).
- `site/demo.html` — product demo page (not part of the design passes).
- `Design-Pass-Changelog.md` — every change since the 11 Aug handoff, with rationale.

## Preview
```bash
cd landing-page/site && python3 -m http.server 5174
```
