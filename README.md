# OpenBerg GitHub Pages

This repository publishes the static web build for **OpenBerg Terminal**.

- Site: [OpenBerg Terminal](https://krkr5628.github.io/openberg.github.io/)
- Public API gateway: `https://api.my-worker-data.workers.dev`
- Current deployment mode: GitHub Pages frontend -> Cloudflare Worker -> Cloudflare Tunnel -> private OpenBerg backend origin
- Updated: 2026-09-10
- Latest application build: [`13f08e0`](https://github.com/krkr5628/openberg.github.io/commit/13f08e0c4ca2b76e4b189f87340408a9fd23348c), published on the existing `main` branch.

**This is a frontend publication repository.** It contains generated web assets and public documentation. Backend services, AI server code, deployment secrets, raw datasets and desktop installers are maintained separately and are not published here.

## What This Build Contains

OpenBerg is a Bloomberg-style dense financial terminal prototype. The current web build includes:

- Docked workspaces with saved layouts, a command palette and linked symbol context
- Watchlist, scanner, chart, company detail, SEC filings, news, portfolio, order ticket, connector health, ontology, graph traversal, AIP query, world map, and strategy/runtime panels
- Chart drawing, indicators, replay, comparisons and image sharing
- Valuation and financial scenario workbenches, custom metric authoring, quantitative research and portfolio risk panels
- Research evidence, review workflows, change inboxes and alert configuration
- Responsive layouts for large displays and mobile viewing
- Korean/English test-model notice popup on first entry
- Market data and AI requests routed through the public Cloudflare gateway; provider access and AI inference run on the backend
- Desktop integration controls shared with the web UI; broker secret configuration requires the separate Electron desktop runtime
- Masked sensitive fields with reveal controls

These are frontend capabilities, not a guarantee that every connected service is available. Some workbenches require compatible backend routes, permissions or prepared data. Missing inputs and unavailable services are displayed explicitly. Analytical libraries that have not been connected to a screen are not presented as released UI features.

## Source Project

The static files in this repository are generated from the OpenBerg frontend workspace:

```text
apps/frontend
```

Build command:

```bash
npm test --prefix apps/frontend
VITE_OPENBERG_BASE_PATH=/openberg.github.io/ \
VITE_OPENBERG_API_BASE=https://api.my-worker-data.workers.dev \
  npm run build --prefix apps/frontend
```

Run these commands in the separately maintained source project. The generated `apps/frontend/dist/` output is published here after validation. `404.html` mirrors `index.html` so client-side routes can load correctly on GitHub Pages. Existing `/risk/` pages are preserved.

`openberg-pages-release.json` records the publication timestamp, predecessor commit, public API base and artifact digest. The digest covers the sorted path-to-SHA-256 mapping of all published files except the release record itself, serialized as compact JSON. Documentation changes therefore also refresh this record.

## Runtime Architecture

```text
Browser
  -> GitHub Pages static assets
  -> Cloudflare Worker API gateway
  -> Cloudflare Tunnel
  -> Private OpenBerg backend
  -> Prepared data, research services and AI providers
```

The deployed API base is the public Worker endpoint. AI queries use the same gateway and backend route; the browser does not contain an AI provider key or run the server's AI service. Frontend calculations and visualization can still run locally in the browser.

The deployed HTML restricts `connect-src` to the same origin and the configured Cloudflare HTTPS/WSS endpoint. Backend authentication and access control remain server responsibilities.

## Referenced Data

The web UI receives prepared data through the backend API. Referenced data families include:

- Financial, market, news, filing and cached observations
- Non-sensitive default preferences and runtime status
- Ontology snapshots and graph projections
- SEC financial facts and filing-derived company data
- KIS/HANKOOK market and account route contracts
- GDELT article, entity, edge, and news-cache datasets
- DART disclosure signal data where available
- Strategy and runtime evaluation results

The frontend does not store server-side source data. It renders responses returned by the backend.

## Security And Privacy Boundary

Publicly exposed in this repository:

- Static HTML/CSS/JS assets
- Public Worker API base URL: `https://api.my-worker-data.workers.dev`
- UI labels, route names, panel names, and non-secret defaults
- Public SEC citation links and library/documentation references
- Development-only loopback fallbacks and URL-parsing/example placeholders; these are not the deployed backend origin

Not intentionally exposed in this repository:

- Cloudflare tunnel token
- Cloudflare account or zone secrets
- OCI private host details
- Backend environment secrets
- HANKOOK/KIS API keys, app secrets or real user account numbers
- GitHub personal access tokens

The former broker-credential cookie storage has been removed. The shared UI clears the legacy cookie and consent marker. Broker key/secret configuration requires Electron IPC and the separate desktop application's operating-system vault; it is unavailable in a normal Pages browser. Public non-secret defaults, such as product type codes, are not credentials.

The 2026-09-10 static exposure review checked all 254 files in application commit `13f08e0` and 57 deleted predecessor assets for key/token patterns, private origin references, sensitive filenames and filesystem paths. No actual secret or private server origin was identified. Loopback addresses occur in non-browser/development fallbacks, SEC URLs are citations, and identity/URL-parser hosts are placeholders. This scoped review does not certify backend responses, infrastructure security or the entire repository history.

## Recent Deployment Notes

2026-09-10:

- Published the current frontend build on the existing `main` branch; preserved prior history and `/risk/` pages.
- Updated the gateway URL, source layout, feature overview, credential-storage description and validation results in this README.
- Restricted credential handling for public GET/HEAD routes to same-origin cookies while preserving explicit settings and private/mutation request behavior.
- Verified Pages deployment and matched the live entry asset to the reviewed build's SHA-256.
- Some deployed backend routes returned 404/500 and some authenticated reads had CORS failures during validation. These remain service integration work; a successful static deployment does not resolve them.
- No backend/AI service release, desktop package publication or source-data processing was performed.

2026-06-04:

- Deployed GitHub Pages build from the OpenBerg frontend.
- Added CORS preflight support for non-safelisted request headers.
- Tuned freshness warning behavior for current snapshot age.
- Added large-display scaling for high-resolution screens.
- Added Korean/English test-model notice popup.
- Added header copyright/data-source notice for SEC, GDELT, KIS/Hankook, DART, and user-managed data.
- Promoted the copyright/data-source notice to a first-run required acknowledgement with an unread-style header badge.
- Reduced Cloudflare Worker request volume by disabling heavy background ontology diagnostics on initial load, slowing heartbeat polling, lazy-loading command metadata, and caching AIP status.
- Removed stale hashed assets from the Pages repository so only the current build files remain published.

## Validation

Frontend checks for application build `13f08e0` (2026-09-10):

```text
npm test --prefix apps/frontend
858 passed: flow 552 / chart 73 / structure 224 / workbench 9

npm run build --prefix apps/frontend
TypeScript, Vite production build and runtime truth checks passed (482 modules)

Sensitive scan: passed (788 source/public/build files)
Desktop viewport, mobile viewport and command-palette smoke: passed
Pages deployment and live release/entry-asset verification: passed
```

Browser smoke covered the frontend shell and command palette. It did not establish that all data, AI, trading or authentication workflows are operational. Documentation-only updates retain these application-build results and are checked separately for publication integrity.
