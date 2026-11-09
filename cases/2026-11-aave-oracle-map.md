# What Aave sees when it prices collateral, a map of Ethereum Core

Read 9 November 2026 at Ethereum block 26,375,420.

## Summary

Aave V3 Ethereum Core holds 21.73B of collateral that can be borrowed against. 58.9% of it is priced by a market feed for the asset itself. The other 41.1%, 8.93B, is priced by something else. A protocol's own exchange rate multiplied by ETH or BTC, or the feed of a different asset altogether. For that 8.93B a discount of the collateral to its reference never reaches a health factor, because the oracle never looks at the collateral's own market.

This note maps where that is and who sits on it. Across the twelve largest assets priced that way, the 50 largest depositors of each carry 7.43B of debt, 72% of it in an E-Mode that lends against the asset at liquidation thresholds of 92% to 95%. At market prices 5% below what the oracle shows, 125 of those accounts, with 3.73B of debt, would be below health factor 1 while the protocol shows them healthy. Nothing in the protocol would move, which is the point.

## How Aave prices each group

| What prices the collateral | Collateral that can be borrowed against | Share |
|---|---|---|
| A market feed for the asset itself (ETH, WBTC, USDT, USDC, DAI, AAVE, LINK, EURC and smaller ones) | 12.80B | 58.9% |
| The protocol's exchange rate times ETH / USD (weETH, wstETH, rsETH, osETH, rETH, cbETH, ETHx) | 6.30B | 29.0% |
| BTC / USD, with no feed for the asset (cbBTC, tBTC, FBTC, BTC.b) | 1.46B | 6.7% |
| The protocol's exchange rate times BTC / USD (LBTC, eBTC) | 0.18B | 0.8% |
| The protocol's exchange rate times USDT / USD (sUSDe, syrupUSDT) | 0.29B | 1.3% |
| USDT / USD, for USDe | 0.64B | 3.0% |
| XAU / USD, for XAUt | 0.06B | 0.3% |

The adapters labelled Capped (CAPO) put a ceiling on how fast an exchange rate may grow. They bound an exchange rate that is pushed up, and do nothing about a collateral that trades below its exchange rate. For the BTC group, Aave uses BTC / USD for cbBTC, tBTC, FBTC and BTC.b with no feed for the wrapped asset, a choice BGD argued for at listing because it trusts the issuer's reserves and avoids DEX manipulation.

## What each group does not see

- Exchange rate times ETH / USD. The oracle sees the stake, not the token. If weETH or rsETH trade below their exchange rate, every position stays where it was.
- BTC / USD with no feed. The oracle sees Bitcoin, not the wrapper. [Case 3](2026-11-tbtc-aave-redemption-pause.md) covers tBTC, where redemption is paused and the oracle still prints BTC.
- Exchange rate times USDT / USD. The oracle sees the vault's share price, not what the share sells for. sUSDe and syrupUSDT can only leave through their own redemption queues or a secondary market.
- USDT / USD for USDe. USDe is priced as if it were USDT. Its own market is read by nothing in the pricing path.

## Who sits on it

The 50 largest aToken holders of each asset at the stated block, their total debt and the E-Mode they are in. The sixth column says how much of those accounts' collateral is the asset itself, since an account can hold several. The E-Mode threshold applies to the asset only when the asset is in that category's collateral list, which is what the seventh column counts.

| Asset | Supplied | Held by the 50 largest | Of them borrowing | Their debt | The asset's share of their collateral | In an E-Mode that includes the asset | Debt in that E-Mode | Lowest health factor | Oracle against market today |
|---|---|---|---|---|---|---|---|---|---|
| weETH | 3,201M | 94% | 50 | 2,671M | 90% | 38 | 2,352M | 1.001 | +0.09% |
| wstETH | 2,041M | 70% | 47 | 1,701M | 46% | 9 | 924M | 1.012 | +0.10% |
| cbBTC | 1,324M | 90% | 50 | 708M | 79% | 0 | 0M | 1.068 | +0.11% |
| rsETH | 672M | 100% | 32 | 616M | 100% | 17 | 616M | 1.010 | +0.35% |
| USDe | 645M | 100% | 33 | 617M | 89% | 31 | 614M | 1.002 | -0.02% |
| osETH | 284M | 98% | 50 | 256M | 100% | 49 | 256M | 1.020 | +0.36% |
| sUSDe | 187M | 100% | 42 | 489M | 34% | 40 | 489M | 1.011 | -0.02% |
| LBTC | 179M | 99% | 47 | 93M | 80% | 1 | 0M | 1.071 | +0.41% |
| tBTC | 119M | 89% | 49 | 85M | 54% | 0 | 0M | 1.116 | +0.14% |
| syrupUSDT | 106M | 100% | 13 | 79M | 100% | 12 | 79M | 1.010 | -0.02% |
| rETH | 87M | 92% | 48 | 76M | 47% | 2 | 1M | 1.029 | +0.06% |
| XAUt | 55M | 94% | 49 | 36M | 73% | 42 | 34M | 1.130 | +0.47% |

The three largest groups are weETH, wstETH and rsETH, together 4,987M of debt, most of it ETH borrowed against the staked version of ETH at a 95% threshold. The lowest health factor among the weETH accounts is 1.001.

USDe is the group that moved most in the four days before this read. Supply went from 219M on 5 November to 645M, and one account, `0xa711A3a6…`, now carries 382M of debt against 358M of USDe and 67M of other collateral in the sUSDe stablecoin E-Mode, at a health factor of 1.024. That one position is priced at USDT / USD on the USDe side.

## What a discount the oracle does not see would do

Model. The asset trades X% below the oracle, every other price and every debt stay where they are, and the liquidation threshold is the one the account actually has, E-Mode or reserve. The table counts accounts whose health factor would be below 1 at that market price and the debt they carry. In the protocol nothing happens at any of these levels. This is what the protocol would be carrying, not what it would liquidate.

Accounts below health factor 1 at market price / their debt.

| Asset | 2% | 5% | 10% | 20% |
|---|---|---|---|---|
| weETH | 12 / 101M | 29 / 2,208M | 35 / 2,328M | 36 / 2,334M |
| wstETH | 1 / 12M | 5 / 61M | 7 / 236M | 10 / 294M |
| cbBTC | 0 / 0M | 0 / 0M | 2 / 21M | 7 / 42M |
| rsETH | 1 / 37M | 12 / 568M | 14 / 616M | 14 / 616M |
| USDe | 1 / 0M | 13 / 526M | 18 / 528M | 23 / 529M |
| osETH | 22 / 114M | 46 / 232M | 46 / 232M | 49 / 256M |
| sUSDe | 1 / 4M | 14 / 54M | 22 / 100M | 28 / 483M |
| LBTC | 0 / 0M | 0 / 0M | 0 / 0M | 3 / 12M |
| tBTC | 0 / 0M | 0 / 0M | 0 / 0M | 2 / 1M |
| syrupUSDT | 1 / 0M | 5 / 78M | 7 / 78M | 8 / 78M |
| rETH | 0 / 0M | 1 / 0M | 1 / 0M | 1 / 0M |
| XAUt | 0 / 0M | 0 / 0M | 0 / 0M | 9 / 3M |

At 2% the weETH positions already start to cross, 39 accounts and 0.27B across the twelve assets. At 5% it is 125 accounts and 3.73B, 2,208M of it against weETH and 526M against USDe.

## Where the collateral would no longer cover the debt

Same model, counting accounts whose collateral at market price is below their debt, and the gap.

| Asset | Accounts at 20% | Shortfall at 20% | Accounts at 30% | Shortfall at 30% |
|---|---|---|---|---|
| weETH | 36 | 285M | 37 | 528M |
| wstETH | 7 | 27M | 8 | 54M |
| cbBTC | 0 | 0M | 5 | 1M |
| rsETH | 14 | 77M | 14 | 145M |
| USDe | 11 | 45M | 21 | 98M |
| osETH | 49 | 33M | 49 | 61M |
| sUSDe | 19 | 8M | 25 | 19M |
| syrupUSDT | 7 | 9M | 8 | 17M |
| rETH | 1 | 0M | 1 | 0M |

Across the twelve assets the gap is 484M at 20% and 923M at 30%. Set against the 7.43B of debt in this sample, it is small. The protocol's exposure is not that the collateral vanishes, it is that 3.73B of positions would be carried with no margin for as long as the discount lasts, with no liquidation to close them and no incentive for anyone to.

## What this says

- The question for 41% of Aave's collateral is not whether the oracle is right about ETH or BTC. It is whether the token in the reserve is worth its exchange rate, and the protocol has decided not to ask.
- E-Mode concentrates the answer. 72% of the debt in this sample sits at 92% to 95% thresholds, so a 5% discount is already past the line for most of it.
- For the LST and LRT group the exit is deep and the exchange rate is backed by a withdrawal queue, so a discount tends to close. For the BTC wrappers and the yield vaults, the exit is whatever redemption the issuer allows that week, which is what case 3 is about.
- USDe is now the second largest group by debt at risk at 5%, and most of it is one account. Whether that is a problem depends on one thing the oracle does not read, the USDe market price.

## Limits

The classification is by the adapter each reserve points to at the stated block. The holder lists come from Blockscout and cover the 50 largest aToken holders of each asset, balances and account data are read on chain at the block. The debt column is each account's total debt, not the part backed by the asset, which is why the collateral share column is there. The model moves one price at a time and holds everything else, so it says what one discount does to one group, not what a correlated move does to all of them. Market prices are DefiLlama's, labelled as such. The E-Mode categories in use were read from the Pool at the block, with their liquidation thresholds and collateral bitmaps.

## Sources

- Aave V3 Pool `0x87870Bca3F3fD6335C3F4ce8392D69350B4fA4E2` (`getReservesList`, `getReserveData`, `getUserAccountData`, `getUserConfiguration`, `getUserEMode`, `getEModeCategoryCollateralConfig`, `getEModeCategoryCollateralBitmap`, `getEModeCategoryLabel`), AaveOracle `0x54586bE62E3c3580375aE3723C145253060Ca0C2` (`getAssetPrice`, `getSourceOfAsset`), each adapter's `description`.
- aToken balances and `totalSupply` for each reserve, holder lists from Blockscout.
- DefiLlama coins API for market prices.
