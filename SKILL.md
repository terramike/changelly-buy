---
name: "changelly_buy"
description: "Help someone buy crypto (default XRP) with a credit/debit card via Changelly: spot-price estimates, attributed buy-link generation, and honest fee disclosure. Use when the user wants to buy crypto with fiat, set up a card on-ramp, or share a buy link with their audience."
---

# Changelly Buy

## Purpose
Let people buy crypto — mainly XRP — with a credit/debit card through Changelly, using the individual-friendly affiliate program (no business entity required). The skill quotes an estimate and generates an attributed buy link; the buyer always completes KYC and payment on Changelly itself.

## Affiliate disclosure
Buy links carry affiliate attribution **by default** (the maintainer's
Changelly referral ID is baked into `bin/changelly-buy` as `DEFAULT_REF_URL`).
Set `CHANGELLY_REF_URL` to your own link — pasted verbatim from your
Changelly affiliate dashboard — to attribute buys to yourself instead.
The skill reports `attributed_to` on every `link` call, so the attribution
is never hidden.

## Workflow
1. **Quote.** Run `bin/changelly-buy quote --crypto <sym> --fiat <code> --fiat-amount <n>` for a spot-price estimate. Defaults: `--crypto xrp --fiat usd`.
2. **Build the link.** Run `bin/changelly-buy link --crypto <sym> --fiat <code> --fiat-amount <n>` to get a buy URL with fiat, crypto, and amount prefilled (`from`/`to`/`amount` params — verified against a live Changelly link). Attribution is the maintainer's default unless `CHANGELLY_REF_URL` is set.
3. **Present.** Give the user: the estimated crypto amount, the buy link, and the fee/KYC disclosure below. If the buyer's wallet is an exchange account (not self-custody), remind them to include the destination tag / memo their exchange requires — Changelly's form has a field for it.

## Output Contract
Every buy offer states all four, in plain language:
- The estimated amount of crypto for the fiat amount (and that it is an estimate).
- The buy link.
- That Changelly (via its payment providers) sets the final price and fees, shown on Changelly before payment — card buys carry provider fees plus a spread over spot.
- That the buyer completes identity verification (KYC) and card entry on Changelly; 18+, subject to their country's availability.

## Operating Rules
1. Never ask for or handle card numbers, IDs, or KYC documents. The buyer enters everything on Changelly.
2. The quote is a CoinGecko spot estimate, not Changelly's price. Never present it as the final price.
3. Never invent referral URL formats. Attribution comes from `CHANGELLY_REF_URL` (verbatim from the affiliate dashboard) or the baked-in `DEFAULT_REF_URL`. See `references/affiliate.md`.
4. Do not claim the purchase is complete. The skill hands off a link; completion happens on Changelly.
5. Fee, limit, and payout details live in `references/` — do not restate them from memory if they conflict with those files.

## Pairs with
The `xrpl` trading skill (https://github.com/terramike/xrpl-muse-skill): once bought XRP lands
in the buyer's wallet, that skill takes over for on-ledger trading via its
propose → approve → sign ceremony. This skill never touches that flow.
