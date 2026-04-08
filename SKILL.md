---
name: clipx-bnb-defi
description: BNB Chain intelligence & DeFi agent — real-time analytics (TVL, fees, meme tokens, DEX volume, market insight) combined with on-chain actions via purr TEE wallet (PancakeSwap swaps, Four.meme buys, Lista deposits, token transfers).
metadata: { "openclaw": { "emoji": "🟡", "requires": { "bins": ["python", "purr"] }, "os": ["linux"] } }
---

## Response rules (read first)

**Rule 1 — Menu format:** Always use numbered lines (1. 2. 3. …). See "Interactive menu" section.

**Rule 2 — Table format:** Always wrap analytics table output inside a markdown code block (triple backticks). Start with a line containing only three backticks, then the table lines, then a line containing only three backticks. This is required so the table displays in monospace with aligned columns. Most API tables are **~40 chars per line** (mobile-friendly); **option 7 (Meme Rank)** may be **~62 chars/line** (MCAP and B.HOLDERS). Do not reformat or truncate — pass through exactly. **Exception:** For option 11 (Binance Announcements), do NOT wrap in code block — pass through the API output as-is (plain markdown with **📢 Binance Announcements** header, 🔸 bullets, and blank line after each item).

**Rule 3 — Confirmation before on-chain actions.** For ANY on-chain operation (options 14–18), you MUST show the user exactly what will happen (token, amount, recipient, action) and ask for explicit confirmation BEFORE running the `purr` command. Never execute a transaction without the user saying "yes" or "confirm".

**Rule 4 — Response ends with the output.** After the table, wallet info, or transaction result, your message is complete. Write nothing else.

**Rule 5 — `purr` must be real, never invented (options 13–18).**
- For wallet and on-chain actions you MUST run the **`purr` shell commands** listed in Section B exactly as written (same flags and order where multiple steps are given).
- **Never** use `api_client_cli.py`, Python, or ClipX API for options 13–18.
- **Never** invent, guess, or template a wallet address (e.g. do not use `0x000…0` unless `purr` actually printed it). **Never** invent balances or transaction hashes.
- Use the **actual stdout (and stderr on failure)** from each `purr` invocation. Summarize only in addition to quoting key facts from real output (address, balance, tx hash).
- If `purr` is missing or returns an error, **say so** and paste the error — do not fabricate success.
- **Option 13 only:** The hex `0x0000000000000000000000000000000000000000` is the null address. A healthy TEE `purr` wallet on BNB Chain is a normal `0x…` address. If you did **not** just receive that null string from `purr wallet address` stdout, **never** show it as the user’s wallet — that is a telltale hallucination. Absurd balances (e.g. tens of thousands of BNB) without raw `purr wallet balance` output are also invalid.

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
3. **For options 1–10, 12:** Put it inside a markdown code block (three backticks on a line before, three backticks on a line after). Most tables are **~40 chars per line**; **option 7** is **~62 chars/line** (MCAP, B.HOLDERS). Pass through exactly — do not reformat, truncate, or drop columns.
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

### Meme Rank — option 7

The API may return a **wider table** with **MCAP** and **B.HOLDERS** (~62 chars/line). Pass through stdout **exactly** in a code block — do not summarize into a shorter 4-column table.

---

## SECTION B — DeFi action commands (13–18)

These commands use the `purr` CLI for on-chain operations via the TEE wallet. Official reference: [Pieverse `purr` commands](https://docs.pieverse.io/cli/commands).

### Quick reference — map number to `purr` commands

| # | Menu label | After user confirmation (where required) | `purr` commands to run |
|---|------------|------------------------------------------|-------------------------|
| 13 | Wallet Info | (no tx — read only) | 1) `purr wallet address --chain-type ethereum` 2) `purr wallet balance` |
| 14 | PancakeSwap | User said yes to swap summary | `purr pancake swap --execute` |
| 15 | Four.meme | User said yes | `purr fourmeme buy --execute` |
| 16 | Lista | List: no deposit yet. Deposit: user said yes | List: `purr lista list-vaults`. Deposit: `purr lista deposit --execute` |
| 17 | Transfer | User said yes | `purr wallet transfer --to <address> --amount <amount> --token <token>` |
| 18 | Bitget | User said yes | `purr bitget swap --execute` |

Use **`purr --help`** or subcommand help on the instance if the CLI prompts for extra flags (pair, slippage, etc.); the base commands above are the Pieverse-documented entry points.

### Option 13 — Wallet Info

Show the user's TEE wallet address and balance **from `purr` only** — same runtime as options 14–18 (if buys show a real `0x…` address elsewhere, option 13 **must** reproduce that address via step 1 below, not from chat memory or a custom card).

**Steps:**

1. Run: `purr wallet address --chain-type ethereum`
2. Run: `purr wallet balance`

**Mandatory reply shape (stops wrong `0x000…` / fake balances):**

1. Put the **full verbatim stdout** from step 1 in a **fenced code block** (label it e.g. `purr wallet address`).
2. Put the **full verbatim stdout** from step 2 in a **second fenced code block** (`purr wallet balance`).
3. **One short line** restating address + balance — every digit must appear in those two blocks (copy-paste substrings only).

**Forbidden for option 13:**

- Hand-typing lines like `Address: …` / `Balance: …` or a “🧾 WALLET INFO (TEE)” card **unless** those values are literally copied from the two stdout blocks above.
- Filling in address or balance from an earlier message without re-running both commands in this turn.
- Proceeding with a summary if either command did not run or failed — instead show stderr / explain the host cannot run `purr`.

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

## Additional `purr` commands (not on menu 1–18)

If the user explicitly asks to **sign** or **run a steps file**, use the official commands (still follow confirmation and Rule 5):

- Message sign: `purr wallet sign --message "<text>"`
- EIP-712: `purr wallet sign-typed-data --data '<json>'` (valid JSON per user / dApp)
- Batch steps: `purr execute --file <path-to-steps.json>`

Solana swaps via DFlow (`purr dflow swap --execute`) are documented on Pieverse but **not** part of this skill’s numbered menu; only use if the user clearly asks for Solana/DFlow.

---

## Other intelligence modes

- `--mode metrics_block --blocks 100` — average block time and gas over recent blocks.
- `--mode metrics_address --address 0x...` — BNB balance and tx count for an address.

---

## Environment

**ClipX API (intelligence, options 1–12, plus metrics):** defaults to `https://skill.clipx.app`. Override with `CLIPX_API_BASE`.

**`purr` (options 13–18):** On [Purrfect Claw hosted runtime](https://docs.pieverse.io/cli/hosted-runtime), `WALLET_API_URL`, `WALLET_API_TOKEN`, and `INSTANCE_ID` are injected — no manual `purr config`. On [external OpenClaw](https://docs.pieverse.io/cli/external-runtime), set `purr config` or the same env vars per [Core concepts](https://docs.pieverse.io/getting-started/core-concepts).
