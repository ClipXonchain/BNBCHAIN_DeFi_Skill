---
name: clipx-bnb-defi
description: BNB Chain intelligence & DeFi agent — real-time analytics (TVL, fees, meme tokens, DEX volume, market insight) combined with on-chain actions via purr TEE wallet (PancakeSwap swaps, Four.meme buys, Lista deposits, token transfers).
metadata: { "openclaw": { "emoji": "🟡", "requires": { "bins": ["python", "purr"] }, "os": ["linux"] } }
---

## Response rules (read first)

**Rule 1 — Menu format:** Always use numbered lines (1. 2. 3. …). See "Interactive menu" section.

**Rule 2 — Table format:** Always wrap analytics table output inside a markdown code block (triple backticks). Start with a line containing only three backticks, then the table lines, then a line containing only three backticks. This is required so the table displays in monospace with aligned columns. The API returns tables formatted at **40 chars per line** (mobile-friendly); do not reformat or truncate — pass through exactly. **Exception:** For option 11 (Binance Announcements), do NOT wrap in code block — pass through the API output as-is (plain markdown with **📢 Binance Announcements** header, 🔸 bullets, and blank line after each item).

**Rule 3 — Confirmation before on-chain actions.** For ANY on-chain operation (options 14–18), you MUST show the user exactly what will happen (token, amount, recipient, action) and ask for explicit confirmation BEFORE running the `purr` command. Never execute a transaction without the user saying "yes" or "confirm".

**Rule 4 — Response ends with the output.** After the table, wallet info, or transaction result, your message is complete. Write nothing else.

---

## What this skill does

Two capabilities in one skill:

1. **Intelligence** — Calls the ClipX BNBChain API via `python "{baseDir}/api_client_cli.py"` to fetch real-time BNB Chain metrics, rankings, and market data. The backend handles all scraping.

2. **DeFi Actions** — Uses the `purr` CLI (TEE-secured wallet) to execute on-chain operations: swaps on PancakeSwap, meme token buys on Four.meme, deposits into Lista DAO vaults, token transfers, and multi-chain swaps via Bitget.

The "analyze then act" loop: discover opportunities through analytics, then immediately execute trades — all without exposing private keys.

---

## Interactive menu

When the user says "clipx", "bnbchain", "defi", or asks for BNB Chain reports/actions without specifying which one, output this menu exactly:

Output this menu inside a code block (triple backticks) so it displays as a formatted box:

```
========================================
🟡 ClipX / BNB Chain Intelligence & DeFi
========================================
 ── INTELLIGENCE ──
 1. TVL Rank
 2. Fees Rank (24h/7d/30d)
 3. Revenue Rank (24h/7d/30d)
 4. DApps Rank
 5. Full Ecosystem
 6. Social Hype
 7. Meme Rank
 8. Network Metrics
 9. Market Insight
10. Market Insight (Live)
11. Binance Announcements
12. DEX Volume (24h/7d/30d)
 ── DeFi ACTIONS ──
13. Wallet Info
14. Swap (PancakeSwap)
15. Buy Meme Token (Four.meme)
16. Lista Vaults & Deposit
17. Transfer Tokens
18. Multi-chain Swap (Bitget)
========================================
Reply with a number (1–18)
```

---

## SECTION A — Intelligence commands (1–12)

These commands call the ClipX API via `api_client_cli.py`.

| # | Command |
|---|---------|
| 1 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type tvl_rank --timezone UTC` |
| 2 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type fees_rank --interval 24h --timezone UTC` |
| 3 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type revenue_rank --interval 24h --timezone UTC` |
| 4 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type dapps_rank --timezone UTC` |
| 5 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type fulleco --timezone UTC` |
| 6 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type social_hype --interval 24 --timezone UTC` |
| 7 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type meme_rank --interval 24 --timezone UTC` |
| 8 | `python "{baseDir}/api_client_cli.py" --mode metrics_basic` |
| 9 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type market_insight --timezone UTC` |
| 10 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type market_insight_live --timezone UTC` |
| 11 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type binance_announcements --timezone UTC` |
| 12 | `python "{baseDir}/api_client_cli.py" --mode clipx --analysis-type dex_volume --interval 24h --timezone UTC` |

For 2 (fees), 3 (revenue), and 12 (DEX volume), default to 24h. If the user specifies 7d or 30d, use `--interval 7d` or `--interval 30d`.

### Displaying intelligence results

The client prints a pre-formatted table. Your job:

1. Run the command.
2. Take the stdout output (the formatted table).
3. **For options 1–10, 12:** Put it inside a markdown code block (three backticks on a line before, three backticks on a line after). Tables are formatted at **40 chars per line** (mobile-friendly); pass through exactly — do not reformat, truncate, or widen.
4. **For option 11 (Binance Announcements):** Output the stdout as-is. Do NOT wrap in code block. Pass through exactly.
5. Send it. Done. Your response is complete.

Example output (API uses 40 chars/line for mobile):

```
========================================
🚀 TOP 10 MEME TOKENS BY SCORE
========================================
----------------------------------------
#   | NAME         | —       | SCORE
----------------------------------------
1   | TokenA       | —       | 4.76
2   | TokenB       | —       | 4.61
...
========================================
Source: @ClipX0_
```

Do not reformat. Pass through the 40-char output as-is.

### Network metrics (option 8)

Returns JSON with `latest_block`, `gas_price_gwei`, `syncing`. Summarize in plain language.

### Market Insight (Live) — option 10

Uses API `market_insight_live` — Volume Leaders + Top Gainers + Top Losers in one snapshot.

### Binance Announcements — option 11

Display rules: Run the command, take the stdout output, and output it exactly as received. Do NOT wrap in a code block. The API returns plain markdown with:
- Bold header: **📢 Binance Announcements**
- 🔸 diamond bullet before each announcement
- Blank line after each item

### DEX Volume — option 12

Top 10 DEXs on BNB Chain by trading volume. Supports intervals: 24h (default), 7d, 30d. Data from DefiLlama.

---

## SECTION B — DeFi action commands (13–18)

These commands use the `purr` CLI for on-chain operations via the TEE wallet.

### Option 13 — Wallet Info

Show the user's wallet address and balance.

**Steps:**

1. Run: `purr wallet address --chain-type ethereum`
2. Run: `purr wallet balance`
3. Present the address and balance to the user in a clear summary.

### Option 14 — Swap on PancakeSwap

Swap tokens on BNB Chain via PancakeSwap.

**Steps:**

1. Ask the user which tokens to swap and the amount (e.g. "Swap 0.1 BNB for CAKE").
2. Show a confirmation summary:
   - From token and amount
   - To token
   - Action: PancakeSwap swap on BNB Chain
3. Wait for the user to confirm with "yes" or "confirm".
4. Run: `purr pancake swap --execute`
5. Return the transaction result from the output.

### Option 15 — Buy Meme Token on Four.meme

Buy a meme token on the Four.meme platform.

**Steps:**

1. Ask the user which token to buy and the amount.
2. Show a confirmation summary:
   - Token to buy
   - Amount of BNB to spend
   - Platform: Four.meme
3. Wait for the user to confirm with "yes" or "confirm".
4. Run: `purr fourmeme buy --execute`
5. Return the transaction result from the output.

### Option 16 — Lista Vaults & Deposit

Browse Lista DAO vaults and deposit.

**Steps:**

1. Run: `purr lista list-vaults`
2. Show the available vaults to the user.
3. If the user wants to deposit, ask which vault and the amount.
4. Show a confirmation summary:
   - Vault name/ID
   - Deposit amount
   - Protocol: Lista DAO
5. Wait for the user to confirm with "yes" or "confirm".
6. Run: `purr lista deposit --execute`
7. Return the transaction result from the output.

### Option 17 — Transfer Tokens

Transfer native tokens or ERC-20s to a recipient address.

**Steps:**

1. Ask the user for: recipient address, amount, and token symbol.
2. Show a confirmation summary:
   - Recipient: `<address>`
   - Amount: `<amount> <token>`
   - Network: BNB Chain
3. Wait for the user to confirm with "yes" or "confirm".
4. Run: `purr wallet transfer --to <address> --amount <amount> --token <token>`
5. Return the transaction hash from the output.

### Option 18 — Multi-chain Swap (Bitget)

Swap tokens across EVM chains via Bitget.

**Steps:**

1. Ask the user which tokens to swap, the amount, and the target chain (if cross-chain).
2. Show a confirmation summary:
   - From token, amount, and chain
   - To token and chain
   - Aggregator: Bitget
3. Wait for the user to confirm with "yes" or "confirm".
4. Run: `purr bitget swap --execute`
5. Return the transaction result from the output.

---

## SECTION C — Smart workflows

When the user describes an intent that combines intelligence and action, chain the steps together. Here are the supported workflows:

### "Find top meme tokens and buy one"

1. Run option 7 (Meme Rank) — show the rankings table.
2. Ask the user: "Which token would you like to buy, and how much BNB to spend?"
3. Follow option 15 (Four.meme buy) flow with confirmation.

### "Show me top DEXs and swap tokens"

1. Run option 12 (DEX Volume) — show the rankings table.
2. Ask the user: "Which tokens would you like to swap and how much?"
3. Follow option 14 (PancakeSwap swap) flow with confirmation.

### "Check my balance and deposit into Lista"

1. Run option 13 (Wallet Info) — show address and balance.
2. Run option 16 step 1 — list available Lista vaults.
3. Ask: "Which vault and how much to deposit?"
4. Follow option 16 deposit flow with confirmation.

### "Show market movers and trade"

1. Run option 10 (Market Insight Live) — show gainers and losers.
2. Ask: "Would you like to swap into any of these tokens?"
3. Follow option 14 (PancakeSwap swap) flow with confirmation.

### "Show my wallet and transfer tokens"

1. Run option 13 (Wallet Info) — show address and balance.
2. Ask: "Who do you want to send to, how much, and which token?"
3. Follow option 17 (Transfer) flow with confirmation.

For any other natural-language request that combines analytics with an on-chain action, follow the same pattern: fetch data first, present it, ask the user what to do, confirm, then execute.

---

## Other intelligence modes

- `--mode metrics_block --blocks 100` — average block time and gas over recent blocks.
- `--mode metrics_address --address 0x...` — BNB balance and tx count for an address.

---

## Environment

The API base URL defaults to `https://skill.clipx.app`. Override with `CLIPX_API_BASE` env var.

The `purr` CLI is pre-configured on Purrfect Claw instances. No wallet setup needed — the TEE wallet is injected automatically.
