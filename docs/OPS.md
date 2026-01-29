# Operations (Ops)

This document summarizes how the system is operated in production.

## Job scheduling
- Nightly pipelines run from scripts (cron or a job runner)
- Revaluation jobs run on schedule and/or on demand
- Price alert scans run regularly

## Operational utilities
- DB health checks (ping/info/schema snapshot)
- Safety scans before publishing code
- Feed validation tools for Merchant outputs

## Monitoring
- PM2 process monitoring and logs
- Script logs written to stdout/stderr
- Fail-fast behavior on missing env vars
