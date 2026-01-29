# Data Pipelines

This document summarizes the main pipelines used by Legendary Collectibles.

## Pricing pipeline (example)
Typical pattern:
1. **Import** (CSV/bulk feeds)
2. **Normalize** (clean and map identifiers)
3. **Build daily values** (derive normalized daily prices)
4. **Rollups** (produce “current” and historical snapshots)

Outputs commonly include:
- current price tables for fast UI reads
- daily rollups for charts/history
- snapshots to preserve historical truth

## Market comps pipeline
- Seed comps from prior vendor history
- Merge/normalize with current sources
- Store structured comps for later analytics

## Revaluation pipeline
- Runs on a schedule or via user-triggered jobs
- Generates valuations for collection items
- Creates daily snapshots for trending and reporting
