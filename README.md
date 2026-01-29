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
