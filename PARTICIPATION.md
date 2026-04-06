# Hackathon participation — Purrfect Claw Web3 Skills

This file is a practical checklist for [Purrfect Claw Web3 Skills (DoraHacks bounty #1338)](https://dorahacks.io/hackathon/bounty/1338) and the [Pieverse Skill Store](https://www.pieverse.io/skill-store).

---

## Bounty deliverables (map your work here)

| Requirement | What to submit | Status (tick when done) |
|-------------|----------------|-------------------------|
| Web3 skill on Skill Store | Publish `clipx-bnb-defi` via ClawHub / merchant flow on Pieverse | [ ] |
| TEE wallet for on-chain ops | `SKILL.md` options 13–18 use `purr` CLI (documented in repo) | [ ] |
| Live demo | Record or host demo on **Purrfect Claw trial instance** (provided after you apply) | [ ] |
| GitHub + documentation | This repo: README, SKILL.md, PARTICIPATION.md | [ ] |

---

## 1. GitHub repository

Push **this folder** (`ClipX_DeFi_Skill`) as its own repo (recommended repo name: `clipx-bnb-defi-skill` or similar).

```bash
cd ClipX_DeFi_Skill
git init
git branch -M main
git add .
git commit -m "Initial: ClipX BNB Chain Intelligence & DeFi skill for Pieverse"
```

Create an empty repo on GitHub (no README if you already have one locally), then:

```bash
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

**After push, save the repo URL** — you will paste it into DoraHacks BUIDL and bounty application.

---

## 2. DoraHacks — BUIDL application

1. Open the bounty: [dorahacks.io/hackathon/bounty/1338](https://dorahacks.io/hackathon/bounty/1338).
2. **Apply** → **Submit new BUIDL** (unless you already linked an existing BUIDL).
3. Fill the BUIDL form. Suggested fields:

**Project name (example)**  
`ClipX BNB Chain Intelligence & DeFi`

**One-line description (example)**  
OpenClaw skill combining BNB Chain analytics (ClipX API) with on-chain DeFi via Purrfect Claw’s TEE wallet (`purr`: PancakeSwap, Four.meme, Lista, transfers).

**Detailed description (paste and edit)**  
This submission is a Purrfect Claw / OpenClaw skill (`SKILL.md`) plus a thin Python client (`api_client_cli.py`). The intelligence layer pulls real-time BNB Chain metrics and rankings (TVL, fees, meme tokens, DEX volume, market insight, announcements, etc.) from the ClipX API. The action layer uses the native `purr` CLI so the agent can check wallet balance and execute swaps, Four.meme buys, Lista deposits, and transfers — without handling private keys. Smart workflows chain “analyze then act” in one chat flow. Source and docs: see GitHub README and SKILL.md.

**Repository**  
Your public GitHub URL (root should be this skill folder or README must point to `ClipX_DeFi_Skill` paths clearly).

**Demo**  
After you receive the **trial instance**, add: short screen recording or link showing (1) menu 1–18, (2) an intelligence result, (3) wallet info via `purr`, (4) optional: a confirmed small swap or balance check only.

**Tech stack**  
Python 3.10+, `requests`, OpenClaw skill format, Pieverse `purr` CLI, BNB Chain.

4. Submit the BUIDL, then complete the **bounty application** linking this BUIDL.

---

## 3. Purrfect Claw trial instance

- The bounty text says the trial is **provided** — follow DoraHacks / Pieverse instructions after acceptance or in the hackathon Discord/FAQ.
- On the instance, confirm:

```bash
purr wallet address --chain-type ethereum
purr wallet balance
```

- Install or copy this skill into the agent’s skill directory per [Writing Skills](https://docs.pieverse.io/cli/writing-skills) and [hosted runtime](https://docs.pieverse.io/cli/hosted-runtime).

---

## 4. Pieverse Skill Store — publish the skill

1. **Merchant / publish flow:** Use [Skill Store](https://www.pieverse.io/skill-store) → “Become a Merchant” or your hackathon onboarding if different.
2. **ClawHub** (if that is your publish path):

```bash
cd ClipX_DeFi_Skill
pip install -r requirements.txt
# clawhub CLI per Pieverse / OpenClaw docs
clawhub publish . \
  --slug clipx-bnb-defi \
  --name "ClipX BNB Chain Intelligence & DeFi" \
  --version 1.0.0 \
  --tags latest,bnbchain,defi,trading,analytics,clipx,fourmeme,pancakeswap,lista
```

3. After publish, **copy the Skill Store listing URL** into your BUIDL “demo / links” field.

---

## 5. Local smoke test (before instance)

Intelligence layer only (no `purr` required):

```bash
cd ClipX_DeFi_Skill
pip install -r requirements.txt
python api_client_cli.py --mode metrics_basic
python api_client_cli.py --mode clipx --analysis-type meme_rank --interval 24 --timezone UTC
```

Optional: `CLIPX_API_BASE=https://skill.clipx.app` (default in code).

---

## 6. Demo script (2–4 minutes)

1. Say **“clipx”** or **“defi”** — show the 1–18 menu.
2. Pick **7** (Meme Rank) — show formatted table in chat.
3. Pick **13** — show `purr` wallet address and balance (TEE).
4. Optional: user confirms a **small** test swap or transfer on testnet/mainnet per hackathon rules.
5. Mention: skill is on Skill Store + GitHub.

---

## 7. Links (keep handy)

- Bounty: https://dorahacks.io/hackathon/bounty/1338  
- Skill Store: https://www.pieverse.io/skill-store  
- Purrfect Claw: https://www.pieverse.io/purrfect-claw  
- Docs (getting started): https://docs.pieverse.io/docs/getting-started  
- purr commands: https://docs.pieverse.io/cli/commands  
- Writing skills: https://docs.pieverse.io/cli/writing-skills  

---

## Notes

- **ClipX API** (`https://skill.clipx.app` by default) is a separate hosted backend; this repo only contains the public thin client. Judges can still verify intelligence outputs live.
- **On-chain actions** must be demonstrated on Purrfect Claw with `purr`; do not put keys or tokens in the repo.
