# Architecture Diagram

```mermaid
flowchart LR
  U[User / Collector] -->|Browse / Search| WEB[Next.js Web App]
  U -->|Sign in| AUTH[Clerk Auth]
  WEB -->|Reads/Writes| DB[(PostgreSQL)]
  WEB -->|Images| CF[Cloudflare Images]
  WEB -->|Checkout| STRIPE[Stripe Checkout]

  subgraph Data_Pipelines[Data Pipelines - Node scripts]
    INGEST[Import / Ingest] --> NORM[Normalize / Map IDs]
    NORM --> ROLLUP[Daily Rollups + Current Views]
    ROLLUP --> ALERTS[Price Alerts + Revalue Jobs]
  end

  INGEST --> DB
  NORM --> DB
  ROLLUP --> DB
  ALERTS --> DB

  DB --> WEB

```


# Notes
Postgres is the source of truth for catalog, pricing, collection state, and snapshots.

Pipelines are designed to be repeatable (import → normalize → rollup → serve).

Checkout is handled via Stripe, while media is delivered via Cloudflare Images.

---

## Pricing & Valuation Data Flow

```mermaid
flowchart TB
  %% --- Sources ---
  subgraph SOURCES[External Sources]
    PC[PriceCharting CSV]
    SF[Scryfall API]
    TCG[TCGplayer + Cardmarket feeds]
    EB[eBay (active + sold comps)]
  end

  %% --- Ingest / Normalize ---
  subgraph PIPE[Pipeline Scripts]
    ING[Import / Ingest]
    MAP[Normalize + Map IDs]
    CONF[Confidence + Source Priority]
    ROLL[Rollups (daily)]
  end

  %% --- Tables ---
  subgraph DBT[Postgres Tables]
    MI[(market_items)]
    MEI[(market_item_external_ids)]
    MPC[(market_prices_current)]
    MPD[(market_price_daily)]
    MPS[(market_price_snapshots)]
    MSC[(market_sales_comps)]
    MVD[(market_values_daily)]

    UCI[(user_collection_items)]
    UCIV[(user_collection_item_valuations)]
    UCDV[(user_collection_daily_valuations)]
    JOBS[(user_revalue_jobs)]

    ALERTS[(price_alerts)]
  end

  %% --- App layer ---
  subgraph APP[Next.js App]
    UI[Browse / Detail / Pricing Pages]
    COL[My Collection + Alerts]
  end

  %% --- Source → ingest ---
  PC --> ING
  SF --> ING
  TCG --> ING
  EB --> ING

  %% --- ingest → tables ---
  ING --> MI
  ING --> MEI
  EB --> MSC

  %% --- normalize / mapping ---
  MI --> MAP
  MEI --> MAP
  MSC --> MAP

  %% --- confidence / source priority ---
  MAP --> CONF
  CONF --> MPC

  %% --- rollups / history ---
  MPC --> ROLL
  ROLL --> MPD
  ROLL --> MPS
  ROLL --> MVD

  %% --- valuations / revalue jobs ---
  MVD --> UCIV
  UCI --> UCIV
  UCIV --> UCDV
  JOBS --> UCIV

  %% --- alerts ---
  MPC --> ALERTS
  ALERTS --> COL

  %% --- app reads ---
  MPC --> UI
  MPD --> UI
  MPS --> UI
  MSC --> UI
  UCI --> COL
  UCIV --> COL
  UCDV --> COL
