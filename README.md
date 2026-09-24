# changelly-buy

A Muse skill that helps people buy crypto (default XRP) with a credit/debit
card via Changelly: spot-price estimates, attributed buy-link generation,
and honest fee disclosure.

## Install

```bash
git clone https://github.com/terramike/changelly-buy.git
# copy the skill into your agent's workspace skills directory, e.g.
cp -r changelly-buy ~/.your-agent/skills/changelly-buy
# or wherever your agent loads workspace skills from
```

No API keys needed. Quotes use CoinGecko's free spot-price endpoint.

## Usage

```bash
# Estimate: how much XRP does $100 buy at spot?
bin/changelly-buy quote --fiat-amount 100

# Attributed buy link, prefilled with $100 → XRP
bin/changelly-buy link --fiat-amount 100

# Other pairs work too
bin/changelly-buy quote --crypto btc --fiat eur --fiat-amount 250
```

## Affiliate disclosure

Buy links carry affiliate attribution **by default** — the maintainer's
Changelly referral ID is baked in. Set `CHANGELLY_REF_URL` to your own
referral link (verbatim from your Changelly affiliate dashboard) to
attribute buys to yourself instead. Every `link` call reports
`attributed_to` so the attribution is never hidden.

## Honest boundaries

- Quotes are CoinGecko **spot estimates**, not Changelly's final price.
  Provider fees and spread apply at checkout, shown on Changelly before payment.
- The buyer completes KYC and card entry **on Changelly** — this skill never
  touches card numbers, IDs, or payments.
- Generating a link is not a purchase. Completion happens on Changelly.

## Pairs with

[xrpl-muse-skill](https://github.com/terramike/xrpl-muse-skill) — once the
bought XRP lands in the buyer's wallet, that skill takes over for on-ledger
trading via its propose → approve → sign ceremony.

## License

MIT — see [LICENSE](LICENSE).
