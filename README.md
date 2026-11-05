# DeFi risk notes

Notes on collateral risk in lending markets (Aave, Morpho, Pendle PTs) by Màrius Agustí Palau.

Every note asks one of three questions. What price does the protocol see for the collateral, and what price does the market pay? Can the collateral be sold when it has to be? When a position is liquidated, how does it run and who gets paid?

Each note states the block it was read at and lists the contracts and transactions behind every figure, so anyone can check it against the chain.

## Notes

| Date | Note | What it covers |
|---|---|---|
| 2026-10-30 | [A 60M USDT position on Aave waiting for liquidation](cases/2026-10-binance-exploit-aave-leg.md) | The Binance exploiter's Aave position, why the USDT blacklist does not reach it, how the liquidation was expected to run, and how it ran on 4 November |
| 2026-08-25 | [PT-reUSD liquidations on Morpho, where the collateral went](cases/2026-08-pt-reusd-liquidations-morpho.md) | 33 liquidations in 14 minutes, why 96% of the seized PT never left Morpho, and what that means for exit liquidity on PT collateral |

## How these notes are made

Figures come from direct contract reads at a stated block, transaction receipts and public explorers. Prices are the Aave oracle unless a note says otherwise, and market prices from DefiLlama are labelled as such. When a number is an estimate, the note says how it was built.

The analysis and the writing are mine. The scripts that pull the data were built with AI assistance and are not published here.

Corrections are welcome. Open an issue with the block or transaction you read differently.

## Contact

[LinkedIn](https://www.linkedin.com/in/mariusagusti) · [X](https://x.com/SrAugustII)
