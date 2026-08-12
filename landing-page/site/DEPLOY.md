# Deploying the Stratemark landing page (Firebase Hosting)

One-time setup (any machine with Node):
1. `npm install -g firebase-tools`
2. `firebase login`  (use the Square Peg / project Google account)
3. In the Firebase console, create a project (e.g. `stratemark-landing`).
   Using a project under the hackathon's Google Cloud credits satisfies the
   "must use at least one Google Cloud product" rule with zero ambiguity.

Deploy (from this folder):
1. `firebase use --add`   → pick the project, alias it `default`
2. `firebase deploy --only hosting`

That's it — Firebase prints the live URL. Custom domain: Hosting → Add custom
domain in the console (two DNS records, ~10 minutes).

BEFORE deploying, do the URL swaps in SWAP-POINTS.md.
