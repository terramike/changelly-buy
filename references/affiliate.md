# Changelly Affiliate Program — operator notes

The partner-API route (MoonPay, Transak, Stripe onramp) requires a registered
business entity. Changelly's affiliate program is the individual-friendly
alternative: it is aimed at creators, community managers, and bloggers, not
companies.

## What it is
- A perpetual referral program: share a referral link or embed the no-code
  exchange widget; earn a cut of Changelly's commission on each referred
  transaction.
- Applies to swaps AND outright crypto purchases (fiat card buys included).

## Economics (verify in dashboard; last checked 2026-09-24)
- Up to **50% revenue share** of Changelly's commission per transaction.
- 30-day affiliate cookie.
- Payouts **monthly**, in BTC or ETH only.
- Minimum payout: 0.01 BTC or 0.1 ETH.
- Accepted worldwide; website/social presence recommended (applications can
  be declined — a real audience helps).

## Setup for this skill
1. Apply at Changelly's affiliate page and get approved.
2. Copy your **full referral link verbatim** from the affiliate dashboard.
3. Set it as `CHANGELLY_REF_URL` in the environment (Secure Vault / shell
   profile — it is not a secret, but keep config out of chat).
4. The skill's `link` command preserves the referral query params exactly and
   normalizes the path to `/buy`. Never hand-edit or "fix" the referral URL.

## Buy-link format (verified 2026-09-24 against a live affiliate link)
`https://changelly.com/buy?from=usd&to=xrp&amount=58.9&ref_id=<id>`
- `from` = fiat currency code, `to` = crypto code, `amount` = fiat amount —
  these prefill the buy form.
- `ref_id` = the affiliate ID from the dashboard. Keep it on every link.

## Attribution default (public repo)
This repo's `bin/changelly-buy` bakes the maintainer's referral ID in as
`DEFAULT_REF_URL`, so out-of-the-box links are attributed to the maintainer.
Set `CHANGELLY_REF_URL` to your own verbatim dashboard link to attribute
buys to yourself instead — the skill reports `attributed_to` on every
`link` call.
