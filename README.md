# Legendary Collectibles — Showcase

![Stars](https://img.shields.io/github/stars/troywoldridge/legendary-collectibles-showcase?style=social)
![Last Commit](https://img.shields.io/github/last-commit/troywoldridge/legendary-collectibles-showcase)
![License](https://img.shields.io/github/license/troywoldridge/legendary-collectibles-showcase)

This repository is a **public-safe portfolio showcase** for the **Legendary Collectibles** platform.

It focuses on what matters to reviewers:
- What the platform does
- How the architecture is structured
- How data pipelines run (pricing, imports, rollups, revaluation)
- What problems were solved (scale, correctness, automation)
- How the system is operated (jobs, validation, safety)

> Note: This repo intentionally contains **no secrets** and excludes private integrations/details.

---

## Live project
- https://legendary-collectibles.com

---

## What I built

### Core platform features
- Catalog system for collectibles + shop products
- Pricing pipelines (imports, normalization, rollups)
- Collection tracking + valuation snapshots
- Alerts (price alerts / revalue jobs)
- Commerce integrations (Google Merchant feed tooling)

### Engineering focus
- Postgres-first data model
- Scripted ETL-style pipelines for repeatability
- Production-safe API patterns and validations
- Operational tooling for maintenance and diagnostics

---

## Documentation

- **Architecture:** `docs/ARCHITECTURE.md`
- **Data pipelines:** `docs/PIPELINES.md`
- **Deployment/ops:** `docs/OPS.md`
- **Security & secrets:** `docs/SECURITY.md`

---

## Screenshots / diagrams
- `screenshots/` — UI / admin / pipeline output screenshots
- `diagrams/` — architecture/pipeline diagrams (simple is fine)

---

## Related repos
- `legendary-data-pipeline` — curated public pipeline scripts
- `legendary-utils` — reusable utilities for DB/feeds/diagnostics

---

## Screenshots

### Home
![Home](screenshots/01-home.png)

### Category discovery
![Category discovery](screenshots/02-category-home.png)

### Browse sets
![Browse sets](screenshots/03-browse-sets.png)

### Browse cards
![Browse cards](screenshots/04-browse-cards.png)

### Card detail
![Card detail](screenshots/05-card-detail.png)

### Pricing + trends
![Pricing + trends](screenshots/06-card-pricing.png)

### Cart review
![Cart review](screenshots/07-cart-review.png)

### Stripe checkout
![Stripe checkout](screenshots/08-stripe-checkout.png)

---

## Highlights

### Platform capabilities
- Unified catalog across TCG + collectibles (cards, sets, sealed, figures)
- Collection tracking + valuation snapshots
- Market pricing views with currency display and trend context
- Checkout flow with shipping estimates and Stripe payment processing

### Engineering & ops
- PostgreSQL as the source of truth for product, pricing, and valuation data
- Scripted pipelines for imports, normalization, and rollups
- Validations and safety checks to prevent secret leakage in public repos
- Production monitoring and scheduled jobs (cron/PM2-style workflows)

---

## Architecture diagram
See: [diagrams/architecture.md](diagrams/architecture.md)

---

## Architecture

```mermaid
flowchart LR
  U[User / Collector] -->|Browse / Search| WEB[Next.js Web App]
  U -->|Sign in| AUTH[Clerk Auth]
  WEB -->|Reads/Writes| DB[(PostgreSQL)]
  WEB -->|Images| CF[Cloudflare Images]
  WEB -->|Checkout| STRIPE[Stripe Checkout]

  subgraph Data_Pipelines[Data Pipelines (Node scripts)]
    INGEST[Import / Ingest] --> NORM[Normalize / Map IDs]
    NORM --> ROLLUP[Daily Rollups + Current Views]
    ROLLUP --> ALERTS[Price Alerts + Revalue Jobs]
  end

  INGEST --> DB
  NORM --> DB
  ROLLUP --> DB
  ALERTS --> DB

  DB --> WEB

END_MERMAID

## Notes
Postgres is the source of truth for catalog, pricing, collection state, and snapshots.

Pipelines are designed to be repeatable (import → normalize → rollup → serve).

Checkout is handled via Stripe, while media is delivered via Cloudflare Images.
