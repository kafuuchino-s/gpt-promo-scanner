# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ChatGPT Team/Business promotional discount code scanner. A collection of CLI Python scripts that discover, validate, price, and generate payment links for ChatGPT promo codes by interacting with OpenAI's backend APIs and Clash Verge proxy for geo-switching.

## Commands

```bash
# Install dependencies
pip install -r requirements.txt

# Main operational tools
python auto_scan.py                  # Full auto-scan all regions
python auto_scan.py GB              # Scan specific region with price collection
python auto_scan.py --no-price      # Skip price collection
python auto_scan.py --list          # List supported regions
python auto_scan.py <region> --open # Auto-open Stripe URLs after scan

# Code discovery
python discover_codes.py GB             # Batch-discover codes for a region
python discover_codes.py --cross        # Full-matrix cross-scan (all bases x all countries)
python discover_codes.py --list         # List supported countries
python discover_codes.py --preview      # Preview candidates without validating
python discover_codes.py --auto-scan    # Auto-verify prices after discovery
python discover_codes.py --code-only    # Output only valid code names

# Specialized scans
python partner_scan.py                  # Scan OpenAI official partners
python mega_scan.py                     # Large-scale AI+MSP company enumeration
python mega_scan.py --resume            # Resume interrupted scan
python us_scan.py                       # US company enumeration
python uk_companies_scan.py             # UK Companies House based scan

# Utilities
python verify.py                        # Simple hardcoded verification
python open_stripe.py <code> <country> <token>  # Generate Stripe payment link
```

No test framework, build system, or CI/CD exists.

## Architecture

All scripts are flat root-level Python modules with no package structure. The pipeline flows:

1. **Company enumeration** → candidate code generation (naming convention pattern)
2. **Token validation** via session API → batch eligibility check (ELIGIBLE/EXISTS/not_found)
3. **Node switching** via Clash API (Unix socket) for geo-location
4. **Stripe URL generation** via checkout API → price collection via metadata + exchange rate APIs
5. **Output** to `stripe_urls.txt`, `scan_results.json`, `metadata_cache.json`

### Module Dependencies

- **`config.py`** — Central config loader (env vars > `config.toml` > defaults). All other modules depend on this. Exports `get_token()`, `get_clash_socket()`, `get_proxy_url()`, `get_proxy_group()`, `get_output_dir()`.
- **`discover_codes.py`** — Core eligibility engine. Contains `batch_check()` and `validate_token()`. Imports from `config.py`; imported by `auto_scan.py`, `partner_scan.py`, `mega_scan.py`, `us_scan.py`, `uk_companies_scan.py`.
- **`auto_scan.py`** — Main operational tool. Adds proxy switching + price collection on top of `discover_codes`.
- **`verify.py`** / **`open_stripe.py`** — Standalone scripts, no imports from other project modules.

### Key External APIs

- OpenAI: `/api/auth/session`, `/backend-api/promotions/eligibility/{code}`, `/backend-api/promotions/metadata/{code}`, `/backend-api/payments/checkout`
- Clash (Unix socket): `/configs`, `/proxies`, `/proxies/{name}/delay`, `PUT /proxies/{group}`
- Exchange rates: `open.er-api.com/v6/latest/USD`

### Dependencies

- `curl_cffi>=0.7.0` — TLS fingerprint mimicry (Chrome 136) to bypass Cloudflare
- `tomli>=1.11.0` — TOML parsing for Python <3.11 (3.11+ uses stdlib `tomllib`)

## Configuration

Copy `config.toml.example` to `config.toml` and fill in:
- `[openai]` token (ChatGPT accessToken)
- `[clash]` socket path and proxy group settings
- `[proxy]` HTTP proxy URL

Environment variables override `config.toml` values.

## Key Data Files

- `known_codes.json` — Database of valid/expired codes across 17 countries with pricing
- `config.toml.example` — Configuration template (actual `config.toml` is gitignored)

## Language Notes

README.md is written in Chinese; README_EN.md is the English translation. SUMMARY.md and DISCOVERY_LOG.md document findings and methodology.
