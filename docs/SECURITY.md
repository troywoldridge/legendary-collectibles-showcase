# Security & Secrets

This repo is intentionally **public-safe**.

## Secrets
- Secrets are never committed to git.
- Local secrets belong in `.env` files (ignored by `.gitignore`), or injected via environment variables.

## What is intentionally excluded
- OAuth flows / token exchange logic for private integrations
- Keys, tokens, production hostnames, internal admin endpoints
- Customer/user data dumps

## Safe patterns
- Use `.env.example` with placeholders only
- Validate required env vars at runtime
- Avoid logging tokens (even token lengths) in public-facing code
