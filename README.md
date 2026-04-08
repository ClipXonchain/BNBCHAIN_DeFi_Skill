# ClipX BNB Chain Intelligence & DeFi Skill

> Analyze BNB Chain markets. Act on-chain. One agent, zero key management.

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Purrfect Claw](https://img.shields.io/badge/Purrfect%20Claw-Skill-purple.svg)](https://www.pieverse.io/purrfect-claw)
[![BNB Chain](https://img.shields.io/badge/BNB%20Chain-BSC-yellow.svg)](https://bnbchain.org)
[![Pieverse Skill Store](https://img.shields.io/badge/Pieverse-Skill%20Store-green.svg)](https://www.pieverse.io/skill-store)
[![GitHub](https://img.shields.io/badge/source-ClipXonchain%2FBNBCHAIN__DeFi__Skill-181717?logo=github)](https://github.com/ClipXonchain/BNBCHAIN_DeFi_Skill)

---

## Repository

Public source: **[github.com/ClipXonchain/BNBCHAIN_DeFi_Skill](https://github.com/ClipXonchain/BNBCHAIN_DeFi_Skill)**

---

## Hackathon participation

**Bounty:** [Purrfect Claw Web3 Skills — DoraHacks #1338](https://dorahacks.io/hackathon/bounty/1338)

Use **[PARTICIPATION.md](PARTICIPATION.md)** for the full checklist: GitHub push, BUIDL copy-paste text, Skill Store publish, trial instance demo, and a short demo script.

**Quick checklist**

- [ ] Public GitHub repo with this folder (`git` commands in PARTICIPATION.md)
- [ ] DoraHacks: submit BUIDL and link it to the bounty
- [ ] Purrfect Claw trial instance: verify `purr wallet address` / `balance`
- [ ] Publish skill to Pieverse Skill Store (ClawHub / merchant flow)
- [ ] Record live demo (intelligence + TEE wallet on instance)

---

## Overview

**ClipX BNB DeFi** is a Purrfect Claw skill that combines **real-time BNB Chain analytics** with **on-chain DeFi actions** — all secured by the TEE wallet.

The "analyze then act" loop: discover opportunities through 12 analytics data sources, then immediately execute trades, swaps, and deposits without ever touching a private key.

| Layer | Capabilities |
|-------|-------------|
| **Intelligence** | TVL, fees, revenue, DApps, full ecosystem, social hype, meme rank, market insight, Binance announcements, DEX volume |
| **DeFi Actions** | PancakeSwap swaps, Four.meme meme token buys, Lista DAO vault deposits, token transfers, Bitget multi-chain swaps |
| **Wallet** | Address lookup, balance check — all via purr TEE wallet |

---

## Architecture

```
User
  │
  ▼
Purrfect Claw Agent
  │
  ├── Intelligence ──► api_client_cli.py ──► ClipX API (VPS)
  │                                              │
  │                                    DefiLlama / DappBay / Binance
  │
  └── DeFi Actions ──► purr CLI ──► TEE Wallet ──► BNB Chain
                                        │
                              PancakeSwap / Four.meme / Lista / Bitget
```

- **Intelligence layer:** Thin HTTP client calls the ClipX API. No scraping, no API keys, no Playwright on the client.
- **Action layer:** `purr` CLI commands execute on-chain operations through the TEE-secured wallet. Private keys never leave the hardware enclave.

---

## Menu Options

### Intelligence (1–12)

| # | Analysis | Description |
|---|----------|-------------|
| 1 | TVL Rank | Top 10 protocols by Total Value Locked |
| 2 | Fees Rank | Top 10 by fees paid (24h / 7d / 30d) |
| 3 | Revenue Rank | Top 10 by revenue (24h / 7d / 30d) |
| 4 | DApps Rank | Top 10 DApps by users (7d) |
| 5 | Full Ecosystem | DeFi, Games, Social, NFTs, AI, Infra, RWA leaders |
| 6 | Social Hype | Top 10 social hype tokens |
| 7 | Meme Rank | Top 10 meme tokens by score |
| 8 | Network Metrics | Latest block, gas price, sync state |
| 9 | Market Insight | Binance 24h volume leaders (snapshot) |
| 10 | Market Insight (Live) | Volume + Top Gainers + Top Losers |
| 11 | Binance Announcements | Top 10 newest announcements |
| 12 | DEX Volume | Top 10 DEXs by volume (24h / 7d / 30d) |

### DeFi Actions (13–18)

| # | Action | Description |
|---|--------|-------------|
| 13 | Wallet Info | Check your TEE wallet address and balance |
| 14 | Swap (PancakeSwap) | Swap tokens on BNB Chain via PancakeSwap |
| 15 | Buy Meme Token (Four.meme) | Buy meme tokens on Four.meme |
| 16 | Lista Vaults & Deposit | Browse and deposit into Lista DAO vaults |
| 17 | Transfer Tokens | Send BNB or ERC-20 tokens to any address |
| 18 | Multi-chain Swap (Bitget) | Cross-chain EVM swaps via Bitget |

### Smart Workflows

The agent supports natural-language commands that chain analytics with actions:

- "Find top meme tokens and buy one" — fetches meme rankings, then executes a Four.meme buy
- "Show me top DEXs and swap tokens" — shows DEX volume, then swaps via PancakeSwap
- "Check my balance and deposit into Lista" — shows wallet balance, lists vaults, then deposits
- "Show market movers and trade" — shows gainers/losers, then swaps on PancakeSwap

---

## Quick Start

### On Purrfect Claw (recommended)

1. Install the skill from the [Pieverse Skill Store](https://www.pieverse.io/skill-store).
2. Say **"clipx"**, **"bnbchain"**, or **"defi"**.
3. The agent shows the 1–18 menu.
4. Pick a number or describe what you want in natural language.

`purr` is pre-installed. The TEE wallet is injected automatically. No setup needed.

### On External OpenClaw

1. Register an agent and install `purr` — see [External OpenClaw docs](https://docs.pieverse.io/cli/external-runtime).
2. Install the skill:

```bash
pip install -r requirements.txt
```

3. Load the skill directory into your OpenClaw instance.
4. Verify `purr` works:

```bash
purr wallet address --chain-type ethereum
```

---

## File Structure

| File | Purpose |
|------|---------|
| `SKILL.md` | Skill definition: menu, intelligence commands, DeFi action flows, smart workflows |
| `api_client_cli.py` | Thin HTTP client for ClipX BNB Chain API (analytics only) |
| `requirements.txt` | Python dependencies (`requests` only) |
| `README.md` | This file |

---

## Demo Guide

### Demo 1 — Intelligence to Action

1. Say: "Show me the top meme tokens"
2. Agent runs meme rank analytics and displays the table
3. Say: "Buy the #1 token with 0.01 BNB on Four.meme"
4. Agent shows confirmation (token, amount, platform)
5. Confirm with "yes"
6. Agent executes `purr fourmeme buy` and returns the transaction result

### Demo 2 — DeFi Deposit

1. Say: "Check my wallet balance"
2. Agent shows TEE wallet address and BNB balance
3. Say: "Show me Lista vaults"
4. Agent lists available vaults
5. Say: "Deposit 0.1 BNB into vault X"
6. Agent confirms and executes via `purr lista deposit`

### Demo 3 — Market Insight + Trade

1. Say: "Show market insight live"
2. Agent displays volume leaders, top gainers, top losers
3. Say: "Swap 0.05 BNB for CAKE on PancakeSwap"
4. Agent confirms and executes via `purr pancake swap`

---

## Configuration

| Variable | Description |
|----------|-------------|
| `CLIPX_API_BASE` | ClipX API base URL. Default: `https://skill.clipx.app` |

The `purr` CLI reads its config from environment variables injected by the Purrfect Claw platform (`WALLET_API_URL`, `WALLET_API_TOKEN`, `INSTANCE_ID`).

---

## purr CLI reference (Pieverse documentation)

This section matches the public **Pieverse** command reference. For authoritative updates, use:

- [Command reference — all `purr` commands](https://docs.pieverse.io/cli/commands)
- [Introduction — what `purr` does](https://docs.pieverse.io/docs/getting-started)
- [Core concepts — TEE wallet, instances, credentials](https://docs.pieverse.io/getting-started/core-concepts)
- [Purrfect Claw hosted runtime](https://docs.pieverse.io/cli/hosted-runtime) (wallet injected automatically)
- [External OpenClaw — install & configure `purr`](https://docs.pieverse.io/cli/external-runtime)

**Command pattern** (from docs):

```bash
purr <command> [args]
```

### Wallet

| Action | Command (from docs) |
|--------|----------------------|
| Get address | `purr wallet address --chain-type ethereum` — returns JSON with your wallet address |
| Balance | `purr wallet balance` — native or token balance |
| Transfer | `purr wallet transfer --to <address> --amount <amount> --token <symbol>` |
| Sign text | `purr wallet sign --message "authenticate:user123"` |
| Sign EIP-712 | `purr wallet sign-typed-data --data <json>` |

### Swaps & routing

| Action | Command (from docs) |
|--------|----------------------|
| PancakeSwap (BNB Chain) | `purr pancake swap --execute` |
| Bitget (multi-chain EVM) | `purr bitget swap --execute` |
| DFlow (Solana) | `purr dflow swap --execute` |

### Lista DAO

| Action | Command (from docs) |
|--------|----------------------|
| List vaults | `purr lista list-vaults` |
| Deposit | `purr lista deposit --execute` |

### Four.meme

| Action | Command (from docs) |
|--------|----------------------|
| Buy tokens | `purr fourmeme buy --execute` |

### Batch transactions

| Action | Command (from docs) |
|--------|----------------------|
| Execute steps file | `purr execute --file <steps.json>` |

### Configure `purr` (external / non-hosted)

From [CLI commands — purr config](https://docs.pieverse.io/cli/commands):

```bash
purr config set api-url "https://purr.pieverse.io"
purr config set api-token "<token>"
purr config set instance-id "<id>"
purr config list
```

Environment variables **override** config file values: `WALLET_API_URL`, `WALLET_API_TOKEN`, `INSTANCE_ID`. See [Core concepts — credentials file](https://docs.pieverse.io/getting-started/core-concepts).

### How this skill uses these commands

[SKILL.md](SKILL.md) maps menu options **13–18** to the flows above. In production the **agent** runs the same `purr` invocations; **you** confirm amounts, recipients, and swaps before any `transfer` or `--execute` runs. For extra flags or interactive prompts, use `purr --help` on your instance and recheck the [official command reference](https://docs.pieverse.io/cli/commands).

---

## Publishing to Pieverse Skill Store

### Publish via ClawHub

```bash
cd ClipX_DeFi_Skill
clawhub publish . \
  --slug clipx-bnb-defi \
  --name "ClipX BNB Chain Intelligence & DeFi" \
  --version 1.0.0 \
  --tags latest,bnbchain,defi,trading,analytics,clipx,fourmeme,pancakeswap,lista
```

### Skill Store Categories

- DeFi
- Trading
- On-chain
- Finance

---

## Security

- **TEE wallet:** Private keys are generated and stored inside the Trusted Execution Environment. They never leave the hardware enclave.
- **No secrets in skill code:** The skill contains no API keys, no private keys, no seed phrases.
- **User confirmation required:** Every on-chain transaction requires explicit user confirmation before execution.
- **Thin client:** The analytics layer only makes HTTP GET requests to the ClipX API. No write operations on the intelligence side.

---

## Hackathon submission (Four.Meme AI Sprint)

Track each deliverable in **[PARTICIPATION.md](PARTICIPATION.md)**.

**Bounty:** [Purrfect Claw Web3 Skills](https://dorahacks.io/hackathon/bounty/1338)

**Required deliverables:**
- Web3 skill published to Pieverse Skill Store
- Uses Purrfect Claw’s native TEE wallet for on-chain operations (`purr` in SKILL.md)
- Live demo on the provided Purrfect Claw trial instance
- GitHub repo with documentation (this README + SKILL.md + PARTICIPATION.md)

**What makes this skill unique:**
- Only skill that combines **12 real-time analytics data sources** with **6 on-chain DeFi actions**
- "Analyze then act" pattern — discover opportunities, then immediately trade
- Uses all major purr integrations: PancakeSwap, Four.meme, Lista DAO, Bitget
- Zero API keys needed on the client side
- Mobile-friendly formatted output (40-char monospace tables)

---

## Links

- [Pieverse Skill Store](https://www.pieverse.io/skill-store)
- [Purrfect Claw Docs](https://docs.pieverse.io/docs/getting-started)
- [purr CLI Commands](https://docs.pieverse.io/cli/commands)
- [Writing Skills](https://docs.pieverse.io/cli/writing-skills)
- [ClipX API](https://skill.clipx.app)

---

## License

Skill code in this directory is licensed under the [MIT License](LICENSE). The ClipX API backend (`skill.clipx.app`) is maintained separately and is not part of this repo.
