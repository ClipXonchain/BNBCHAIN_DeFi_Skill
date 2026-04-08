# ClipX BNB Chain Intelligence

> Real-time BNB Chain analytics in your agent — rankings, market insight, network metrics. Thin API client, no keys, no scraping on the client.

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Skill-green.svg)](https://openclaw.ai)
[![BNB Chain](https://img.shields.io/badge/BNB%20Chain-BSC-yellow.svg)](https://bnbchain.org)
[![Pieverse Skill Store](https://img.shields.io/badge/Pieverse-Skill%20Store-green.svg)](https://www.pieverse.io/skill-store)
[![GitHub](https://img.shields.io/badge/source-ClipXonchain%2FBNBCHAIN__DeFi__Skill-181717?logo=github)](https://github.com/ClipXonchain/BNBCHAIN_DeFi_Skill)

On GitHub use branch **`main`**: **[README on repo](https://github.com/ClipXonchain/BNBCHAIN_DeFi_Skill/blob/main/README.md)** · **[raw README](https://raw.githubusercontent.com/ClipXonchain/BNBCHAIN_DeFi_Skill/main/README.md)**

---

## Overview

**ClipX BNB Intelligence** is an OpenClaw / Purrfect Claw–compatible skill: a **thin HTTP client** calls the ClipX BNB Chain API for metrics and rankings. Heavy work (Playwright, aggregation) runs on the API host; this repo only ships `SKILL.md` + `api_client_cli.py`.

| Capability | Description |
|------------|-------------|
| **Rankings** | TVL, fees, revenue, DApps, full ecosystem, social hype, meme rank |
| **Market** | Volume leaders, gainers/losers, Binance announcements |
| **Network** | Block, gas, sync, block stats, address summary |
| **Output** | Pre-formatted tables (mobile-friendly), optional raw JSON |

---

## Architecture

```
User → Agent → api_client_cli.py → ClipX API → DefiLlama / DappBay / Binance / RPC
         ↑__________________________________|
```

---

## Menu (1–12)

| # | Analysis |
|---|----------|
| 1 | TVL Rank |
| 2 | Fees Rank (24h / 7d / 30d) |
| 3 | Revenue Rank (24h / 7d / 30d) |
| 4 | DApps Rank |
| 5 | Full Ecosystem |
| 6 | Social Hype |
| 7 | Meme Rank |
| 8 | Network Metrics |
| 9 | Market Insight |
| 10 | Market Insight (Live) |
| 11 | Binance Announcements |
| 12 | DEX Volume (24h / 7d / 30d) |

---

## Quick Start

1. `pip install -r requirements.txt`
2. Load this folder as an OpenClaw skill (see [Writing Skills](https://docs.pieverse.io/cli/writing-skills)).
3. Say **"clipx"** or **"bnbchain"** → user picks **1–12**.

### Smoke test (local)

```bash
pip install -r requirements.txt
python api_client_cli.py --mode metrics_basic
python api_client_cli.py --mode clipx --analysis-type tvl_rank --timezone UTC
```

---

## File Structure

| File | Purpose |
|------|---------|
| `SKILL.md` | Menu, command mapping, display rules |
| `api_client_cli.py` | HTTP client for ClipX API |
| `requirements.txt` | `requests` |
| `README.md` | This file |

---

## Demo ideas

1. **Meme rank:** User picks `7` → agent runs `api_client_cli.py` with `meme_rank` → show table (may include MCAP / B.HOLDERS).
2. **Market live:** User picks `10` → volume + gainers + losers snapshot.
3. **TVL:** User picks `1` → top protocols by TVL.

---

## Configuration

| Variable | Description |
|----------|-------------|
| `CLIPX_API_BASE` | API base URL. Default: `https://skill.clipx.app` |

---

## Publishing to Pieverse Skill Store

1. [Skill Store](https://www.pieverse.io/skill-store) → **Become a Merchant** (or your onboarding flow), then upload / publish per Pieverse instructions.
2. Example **ClawHub** (if that is your path):

```bash
cd ClipX_DeFi_Skill   # or your clone of BNBCHAIN_DeFi_Skill
clawhub publish . \
  --slug clipx-bnb-intelligence \
  --name "ClipX BNB Chain Intelligence" \
  --version 1.0.0 \
  --tags latest,bnbchain,metrics,analytics,clipx,defi,trading,on-chain
```

Adjust slug/name to match your store listing.

---

## Security

- **No API keys** in the client; public ClipX API endpoints only (per your deployment).
- **No private keys** in this repo — analytics are read-only HTTP GETs.
- **Thin client:** No Playwright or scrapers shipped here.

---

## Links

- [Pieverse Skill Store](https://www.pieverse.io/skill-store)
- [Writing Skills](https://docs.pieverse.io/cli/writing-skills)
- [ClipX API](https://skill.clipx.app)

---

## License

Skill code in this directory is licensed under the [MIT License](LICENSE). The ClipX API backend is maintained separately.
