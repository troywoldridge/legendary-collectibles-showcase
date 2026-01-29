# Architecture

This document describes the high-level architecture of **Legendary Collectibles**.

## Stack (high-level)
- **Frontend:** Next.js (App Router), React
- **Backend:** Next.js route handlers (API), Node.js scripts for pipelines
- **Database:** PostgreSQL
- **ORM:** Drizzle ORM
- **Auth:** Clerk
- **Payments:** Stripe
- **Images:** Cloudflare Images
- **Hosting/ops:** Linux server + PM2 (with scheduled jobs)

## Core components
1. **Web app**
   - Public browsing experience
   - Admin interfaces (catalog, listings, pipelines, etc.)

2. **Database**
   - Canonical source of truth for products, prices, collections, snapshots
   - Snapshot/rollup tables for fast reads and history

3. **Pipelines / Jobs**
   - Imports from external sources
   - Normalization and rollups
   - Revaluation jobs for user collections
   - Price alert scanning

## Design goals
- Correctness > hype
- Repeatable pipelines (idempotent where possible)
- Safe secret handling via environment variables
- Clear separation between:
  - ingestion (import)
  - normalization (cleaning)
  - aggregation (rollups)
  - serving (API/UI)
