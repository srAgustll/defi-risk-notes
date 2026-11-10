# Record

What the notes in this repository said before something happened, and what the chain showed afterwards. Each entry links the commit where the statement was first published, so its date can be checked against the repository history. Open entries stay here until they settle, whichever way they go.

## Settled

### The 60M USDT position on Aave, first liquidation

Stated on 3 November 2026 at 06.28 UTC, in [commit 1931c92](https://github.com/srAgustll/defi-risk-notes/commit/1931c92) of [the note](cases/2026-10-binance-exploit-aave-leg.md), from a read at block 26,325,444 on 2 November.

Settled on 4 November 2026 at 14.49 UTC, 32 hours after publication, at block 26,342,152 in transaction [0xf5ff0c71…](https://etherscan.io/tx/0xf5ff0c7106886a61fddb7a03ab7da7b3ae50cd4ba62e1f418b7a07f98434a3eb).

| Item | Stated | On chain |
|---|---|---|
| Trigger | ETH about 2,366 | Chainlink ETH / USD update to 2,367.21 at index 1 of the block, liquidation at index 2 |
| Path | Half the debt in a first call, not the whole position | Half the debt in one call |
| Debt repaid | 9,883.1 WETH | 9,884.42 WETH |
| Collateral taken | about 24.47M USDT | 24,378,311 USDT |
| Bonus | about 1.05M USD, about 0.1M of it to the Aave treasury | 1.053M USD, 105,590 aUSDT of it to the Aave Collector |
| Health factor after | about 1.18 | 1.184 |
| Next trigger | near ETH 2,800 | ETH 2,803 |

What the note did not anticipate. It left open what a liquidator would do with the USDT. The liquidator sold all of it for WETH inside the same transaction, paying 1.16% above the oracle price of ETH, and that exit cost 389k of the 947.6k USD of bonus it received. The note had no estimate for that cost.

## Open

| Stated | Note | What was said | What settles it |
|---|---|---|---|
| 5 November 2026, [commit ce39152](https://github.com/srAgustll/defi-risk-notes/commit/ce39152) | [The 60M USDT position on Aave](cases/2026-10-binance-exploit-aave-leg.md) | The second half becomes liquidatable at ETH 2,803, and again half the remaining debt goes per call unless the health factor is below 0.95 | The position is liquidated, repaid or withdrawn |
| 6 November 2026, [commit 55411ab](https://github.com/srAgustll/defi-risk-notes/commit/55411ab) | [tBTC on Aave while redemption is paused](cases/2026-11-tbtc-aave-redemption-pause.md) | A tBTC discount to BTC reaches no health factor. If BTC falls 30%, about 245 tBTC enters the first round of liquidations, against an on chain exit that absorbs about 135 tBTC before the liquidation bonus is used up | A liquidation of tBTC collateral while the reserve is frozen, read for what the liquidator does with the tBTC, or redemption reopening first |
| 9 November 2026, [commit 07d01d3](https://github.com/srAgustll/defi-risk-notes/commit/07d01d3) | [What Aave sees when it prices collateral](cases/2026-11-aave-oracle-map.md) | For 41.1% of the collateral a discount to the reference asset reaches no health factor. At market prices 5% below the oracle, 125 accounts with 3.73B of debt would be under health factor 1 and none would be liquidated. One USDe account carries 382M of debt at a health factor of 1.024, priced at USDT / USD | One of those assets trading below its oracle price for long enough to count liquidations, or Aave changing the price source |

Last checked on 10 November 2026 at block 26,382,866. None of the three has settled. The first position sits at a health factor of 1.32 with ETH at 2,117, the tBTC reserve is still frozen, and the USDe account is at 1.024.

## Corrections

No figure published in this repository has been corrected after publication so far. When one is, it is listed here with the commit that changed it.
