# LES Services — Deploy Guide

One file (`index.html`), zero dependencies, zero build step. It runs on any host that can serve a static page.

## Deploy to GoDaddy
1. Log in to GoDaddy → My Products → your hosting plan → **cPanel / File Manager**.
2. Open the `public_html` folder.
3. Upload `index.html`.
4. Visit your domain. Done.

## Deploy to Vercel
Option A (drag and drop): go to vercel.com/new, drag the folder containing `index.html`, click Deploy.

Option B (CLI):
```
npm i -g vercel
cd <folder with index.html>
vercel --prod
```

## Deploy anywhere else
Netlify, Cloudflare Pages, S3, shared hosting, a Raspberry Pi — upload `index.html` and you're live.

## Using the app
- **Consumer side:** landing page → "Start my check" → 7-step friendly wizard → results dashboard with a 0–100 qualification score, indicator band, five-pillar scorecard, rate, payment breakdown, impact-ranked tips, save quote, LES Services document upload, and live-agent callback.
- **Back office:** click "Back office" in the header. Default PIN: **2468** (change it under Settings). Sidebar: Applications (score + pillar bars per file), Rates (publish daily — quotes update in real time), Thresholds (tune weights and limits without code), Callbacks, Settings.

## The five-pillar engine
Each pillar scores 0–100, weighted, gated by hard stops — the standard framework every underwriter applies:

| Pillar | Weight | Full marks | Hard stop |
|---|---|---|---|
| Credit tier | 25% | 740+ | under 580 |
| Debt-to-income | 30% | ≤ 36% back-end | over 50% |
| Loan-to-value | 20% | ≤ 80% | over program cap (97 conv / 96.5 FHA / 80 cash-out) |
| Income stability | 15% | W-2, 2+ years, no gaps | — |
| Assets & reserves | 10% | 6+ months of payments after closing | can't cover cash to close |

Bands: 75+ likely qualifies · 55–74 conditional · under 55 (or any hard stop) needs work. Every threshold and weight above is editable in the back office. Tips are computed by re-running the engine on single changes and ranking by score lift; changes that move the band get a "flips your band" label.

- Rate = your published base rate + credit-tier adjustment + LTV adjustment + cash-out adjustment
- Payment = P&I + est. taxes (1.1%/yr) + est. insurance ($1,500/yr) + mortgage insurance above 80% LTV

This is a qualification indicator, not an approval — the engine mirrors standard agency (Fannie/Freddie/FHA-style) guidelines, so the read reflects what a typical lender would say.

## Phase 2 (production hardening)
Current build stores data in the visitor's browser (private, nothing transmitted). To take real applications:
1. Add a backend (Vercel serverless functions or any Node/PHP host) with a database for applications, rates, and callbacks.
2. Real email verification (e.g., a magic-link on signup).
3. Encrypted document storage (S3/R2) behind authentication for the LES Services uploads.
4. A licensed-lender integration or LOS handoff for actual rate locks.

The UI needs no redesign for any of this — swap the `store` object's localStorage calls for API calls.
