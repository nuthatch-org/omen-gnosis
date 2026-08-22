# omen-gnosis

A [nuthatch](https://github.com/nightswatchhq/nuthatch) nest: **Omen prediction markets on Gnosis**.

Every fixed-product market maker Omen's factory has ever cloned, and the trades on them: buys, sells and liquidity funding.

One binary, one config file, no graph-node, no gateway, no query fees.

## What it indexes

**Chain:** `gnosis`. **1 contract**, **5 tables**.

| alias | address |
|---|---|
| `factory` | `0x9083a2b699c0a4ad06f63580bde2635d26a3eef0` |

## Read this before trusting it

- The factory's own ABI declares the *children's* trade events, because the Solidity that builds a market maker imports them. Decoded on the factory they stand up four permanently empty tables, so the ABI is split.
- `CloneCreated`, inherited from the CloneFactory base, is **never emitted** - measured zero from this address over 5,000,000 blocks and zero anywhere on Gnosis. Dropped rather than shipped as a fifth empty table.

## Run it

```sh
nuthatch init --from https://github.com/nightswatchhq/omen-gnosis
cd omen-gnosis
nuthatch dev --dir . --backfill 50000 --seal-direct
nuthatch sql --dir . "SELECT count(*) FROM \"factory__fixed_product_market_maker_creation\""
```

The endpoint in `nuthatch.toml` is keyless and public, so this file is publishable: a `nuthatch.toml` is pinned into the nest's content address and must never carry a credential. It is enough to follow the tip. A **backfill** wants archive depth it may not have: pass your own with `--rpc`, and check it first with `nuthatch doctor --rpc <url>`.

## Tables

```
factory__fixed_product_market_maker_creation
fpmm__f_p_m_m_buy
fpmm__f_p_m_m_funding_added
fpmm__f_p_m_m_funding_removed
fpmm__f_p_m_m_sell
```
