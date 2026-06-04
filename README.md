# OpenBerg GitHub Pages

This repository publishes the static web build for **OpenBerg Terminal**.

- Site: `https://krkr5628.github.io/openberg.github.io/`
- Public API gateway: `https://api.openberg.workers.dev`
- Current deployment mode: GitHub Pages frontend -> Cloudflare Worker -> Cloudflare Tunnel -> private OpenBerg backend origin
- Last documented deployment date: 2026-06-04

## What This Build Contains

OpenBerg is a Bloomberg-style dense financial terminal prototype. The current web build includes:

- 8 workspace layout with docked panels
- Watchlist, scanner, chart, company detail, SEC filings, news, portfolio, order ticket, connector health, ontology, graph traversal, AIP query, world map, and strategy/runtime panels
- Large-display responsive scaling for 4K desktop use
- Korean/English test-model notice popup on first entry
- Backend-routed data access only; frontend does not directly scrape or call raw third-party data sources
- User-managed HANKOOK/KIS credential entry for optional trading-route testing
- Masked sensitive fields with reveal controls

## Source Project

The static files in this repository are generated from the OpenBerg frontend workspace:

```text
OPENBERG_WEB/DEV_02_Frontend
```

Build command:

```bash
npm run build
```

The generated `dist/` output is copied into this GitHub Pages repository. `404.html` mirrors `index.html` so client-side routes can load correctly on GitHub Pages.

## Runtime Architecture

```text
Browser
  -> GitHub Pages static assets
  -> Cloudflare Worker API gateway
  -> Cloudflare Tunnel
  -> OpenBerg FastAPI backend
  -> DATA_LAKE / ONTOLOGY snapshots / runtime caches
```

The frontend only knows the public Worker endpoint. Cloudflare account IDs, tunnel tokens, origin host details, and backend credentials are not embedded in this static build.

## Referenced Data

The web UI receives data through the backend API. The backend currently references these private data families:

- `DATA_LAKE`: parquet-based financial, market, news, filing, and cache data
- `DATA_CACHE` / default preferences: non-sensitive default UI/runtime cache
- `ONTOLOGY/ONTOLOGY_CONNECTION_V1`: active ontology snapshots and graph projections
- SEC financial facts and filing-derived company data
- KIS/HANKOOK market and account route contracts
- GDELT article, entity, edge, and news-cache datasets
- DART disclosure signal data where available
- Strategy/runtime evaluation artifacts for later INDEX_STRATEGY work

The frontend does not store server-side source data. It renders responses returned by the backend.

## Security And Privacy Boundary

Publicly exposed in this repository:

- Static HTML/CSS/JS assets
- Public Worker API base URL: `https://api.openberg.workers.dev`
- UI labels, route names, panel names, and non-secret defaults

Not intentionally exposed in this repository:

- Cloudflare tunnel token
- Cloudflare account or zone secrets
- OCI private host details
- Backend environment secrets
- HANKOOK/KIS API key, app secret, real account number, or product code values
- GitHub personal access tokens

Sensitive trading inputs are user-managed in the browser. If a user chooses to save HANKOOK/KIS credentials, the current prototype stores them in that user's browser cookie after explicit risk consent. This is convenience storage, not server-side custody.

## Recent Deployment Notes

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

Frontend checks used before deployment:

```text
npm test
flow tests: 17/17
structure tests: 6/6

npm run build
vite production build passed
```
