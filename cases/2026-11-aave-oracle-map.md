# What Aave sees when it prices collateral, a map of Ethereum Core

Read 16 November 2026 at Ethereum block 26,426,794. First read 9 November 2026 at block 26,375,420, and refreshed every Monday.

## Summary

Aave V3 Ethereum Core holds 20.00B of collateral that can be borrowed against. 59.5% of it is priced by a market feed for the asset itself. The other 40.5%, 8.10B, is priced by something else. A protocol's own exchange rate multiplied by ETH or BTC, or the feed of a different asset altogether. For that 8.10B a discount of the collateral to its reference never reaches a health factor, because the oracle never looks at the collateral's own market.

This note maps where that is and who sits on it. Across the twelve largest assets priced that way, the 50 largest depositors of each carry 6.77B of debt, 70% of it in an E-Mode that includes the asset, all but 33M of that at liquidation thresholds of 92% to 95%. At market prices 5% below what the oracle shows, 134 of those accounts, with 3.44B of debt, would be below health factor 1 while the protocol shows them healthy. Nothing in the protocol would move, which is the point.

## Changes since the previous read

Between 9 and 16 November the collateral that can be borrowed against went from 21.73B to 20.00B, and the share priced without the asset's own market from 41.1% to 40.5%. The Aave oracle showed ETH at 2,131.68 on the first date and 1,795.31 on the second.

One reserve was added, cirBTC. Aave prices it with the same BTC / USD source as tBTC, and 62.66 cirBTC were supplied at the block, 4.0M.

The debt of the sample went from 7.43B to 6.77B. At market prices 5% below the oracle, the accounts below health factor 1 went from 125 with 3.73B of debt to 134 with 3.44B. USDe is now the second group at that level, 571M against 467M for rsETH. tBTC supplied went from 119M to 95M and the reserve is still frozen.

## How Aave prices each group

| What prices the collateral | Collateral that can be borrowed against | Share |
|---|---|---|
| A market feed for the asset itself (ETH, WBTC, USDT, USDC, DAI, AAVE, LINK, EURC and smaller ones) | 11.91B | 59.5% |
| The protocol's exchange rate times ETH / USD (weETH, wstETH, rsETH, osETH, rETH, cbETH, ETHx) | 5.48B | 27.4% |
| BTC / USD, with no feed for the asset (cbBTC, tBTC, FBTC, cirBTC, BTC.b) | 1.48B | 7.4% |
| The protocol's exchange rate times BTC / USD (LBTC, eBTC) | 0.15B | 0.7% |
| The protocol's exchange rate times USDT / USD (sUSDe, syrupUSDT) | 0.34B | 1.7% |
| USDT / USD, for USDe | 0.59B | 3.0% |
| XAU / USD, for XAUt | 0.05B | 0.2% |

The adapters labelled Capped (CAPO) put a ceiling on how fast an exchange rate may grow. They bound an exchange rate that is pushed up, and do nothing about a collateral that trades below its exchange rate. For the BTC group, Aave uses BTC / USD for cbBTC, tBTC, FBTC, cirBTC and BTC.b with no feed for the wrapped asset, a choice BGD argued for at listing because it trusts the issuer's reserves and avoids DEX manipulation.

## What each group does not see

- Exchange rate times ETH / USD. The oracle sees the stake, not the token. If weETH or rsETH trade below their exchange rate, every position stays where it was.
- BTC / USD with no feed. The oracle sees Bitcoin, not the wrapper. [Case 3](2026-11-tbtc-aave-redemption-pause.md) covers tBTC, where redemption is paused and the oracle still prints BTC.
- Exchange rate times USDT / USD. The oracle sees the vault's share price, not what the share sells for. sUSDe and syrupUSDT can only leave through their own redemption queues or a secondary market.
- USDT / USD for USDe. USDe is priced as if it were USDT. Its own market is read by nothing in the pricing path.

## Who sits on it

The 50 largest aToken holders of each asset at the stated block, their total debt and the E-Mode they are in. The sixth column says how much of those accounts' collateral is the asset itself, since an account can hold several. The E-Mode threshold applies to the asset only when the asset is in that category's collateral list, which is what the seventh column counts.

| Asset | Supplied | Held by the 50 largest | Of them borrowing | Their debt | The asset's share of their collateral | In an E-Mode that includes the asset | Debt in that E-Mode | Lowest health factor | Oracle against market today |
|---|---|---|---|---|---|---|---|---|---|
| weETH | 2,716M | 94% | 49 | 2,287M | 89% | 36 | 1,976M | 1.001 | +0.04% |
| wstETH | 1,900M | 72% | 47 | 1,498M | 53% | 11 | 830M | 1.023 | +0.08% |
| cbBTC | 1,373M | 90% | 50 | 783M | 83% | 0 | 0M | 1.111 | -0.15% |
| USDe | 593M | 100% | 33 | 591M | 89% | 31 | 588M | 1.002 | -0.02% |
| rsETH | 553M | 100% | 32 | 507M | 100% | 18 | 507M | 1.011 | +0.21% |
| osETH | 238M | 98% | 50 | 215M | 100% | 49 | 215M | 1.020 | +0.30% |
| sUSDe | 233M | 100% | 45 | 521M | 39% | 43 | 521M | 1.011 | -0.01% |
| LBTC | 143M | 99% | 47 | 91M | 75% | 1 | 0M | 1.043 | +0.39% |
| syrupUSDT | 104M | 100% | 13 | 88M | 100% | 12 | 88M | 1.010 | +0.12% |
| tBTC | 95M | 92% | 49 | 82M | 54% | 0 | 0M | 1.055 | +0.12% |
| rETH | 67M | 91% | 49 | 73M | 45% | 5 | 1M | 1.029 | +0.09% |
| XAUt | 50M | 93% | 48 | 35M | 74% | 43 | 33M | 1.059 | +0.66% |

The three largest ETH staking groups, weETH, wstETH and rsETH, carry together 4,292M of debt, 77% of it in an E-Mode that includes the asset, at a 95% threshold. The lowest health factor among the weETH accounts is 1.001.

USDe supply was 593M at this read, against 645M a week earlier and 219M on 5 November. One account, `0xa711A3a6…`, carries 383M of debt against 359M of USDe and 67M of other collateral in the sUSDe stablecoin E-Mode, at a health factor of 1.024, the same as a week earlier. That one position is priced at USDT / USD on the USDe side.

## What a discount the oracle does not see would do

Model. The asset trades X% below the oracle, every other price and every debt stay where they are, and the liquidation threshold is the one the account actually has, E-Mode or reserve. The table counts accounts whose health factor would be below 1 at that market price and the debt they carry. In the protocol nothing happens at any of these levels. This is what the protocol would be carrying, not what it would liquidate.

Accounts below health factor 1 at market price / their debt.

| Asset | 2% | 5% | 10% | 20% |
|---|---|---|---|---|
| weETH | 12 / 92M | 28 / 1,863M | 34 / 1,961M | 35 / 1,966M |
| wstETH | 0 / 0M | 8 / 219M | 9 / 249M | 13 / 298M |
| cbBTC | 0 / 0M | 0 / 0M | 0 / 0M | 7 / 30M |
| USDe | 1 / 0M | 12 / 571M | 20 / 576M | 25 / 582M |
| rsETH | 1 / 31M | 13 / 467M | 15 / 507M | 16 / 507M |
| osETH | 24 / 96M | 47 / 195M | 48 / 196M | 49 / 215M |
| sUSDe | 4 / 6M | 18 / 45M | 25 / 55M | 30 / 513M |
| LBTC | 0 / 0M | 1 / 0M | 1 / 0M | 3 / 2M |
| syrupUSDT | 1 / 0M | 5 / 75M | 7 / 75M | 8 / 75M |
| tBTC | 0 / 0M | 0 / 0M | 3 / 2M | 10 / 20M |
| rETH | 0 / 0M | 2 / 1M | 2 / 1M | 4 / 8M |
| XAUt | 0 / 0M | 0 / 0M | 3 / 0M | 16 / 13M |

At 2% the weETH positions already start to cross, 43 accounts and 0.23B across the twelve assets. At 5% it is 134 accounts and 3.44B, 1,863M of it against weETH and 571M against USDe.

## Where the collateral would no longer cover the debt

Same model, counting accounts whose collateral at market price is below their debt, and the gap.

| Asset | Accounts at 20% | Shortfall at 20% | Accounts at 30% | Shortfall at 30% |
|---|---|---|---|---|
| weETH | 35 | 239M | 36 | 444M |
| wstETH | 9 | 32M | 11 | 60M |
| cbBTC | 0 | 0M | 5 | 1M |
| USDe | 14 | 49M | 23 | 107M |
| rsETH | 15 | 64M | 15 | 119M |
| osETH | 49 | 28M | 49 | 51M |
| sUSDe | 21 | 5M | 29 | 17M |
| LBTC | 0 | 0M | 1 | 0M |
| syrupUSDT | 7 | 8M | 8 | 16M |
| tBTC | 0 | 0M | 3 | 0M |
| rETH | 2 | 0M | 2 | 0M |
| XAUt | 0 | 0M | 2 | 0M |

Across the twelve assets the gap is 425M at 20% and 815M at 30%. Set against the 6.77B of debt in this sample, it is small. The protocol's exposure is not that the collateral vanishes, it is that 3.44B of positions would be carried with no margin for as long as the discount lasts, with no liquidation to close them and no incentive for anyone to.

## What this says

- The question for 40% of Aave's collateral is not whether the oracle is right about ETH or BTC. It is whether the token in the reserve is worth its exchange rate, and the protocol has decided not to ask.
- E-Mode concentrates the answer. 70% of the debt in this sample sits in an E-Mode that includes the asset, nearly all of it at 92% to 95% thresholds, so a 5% discount is already past the line for most of it.
- For the LST and LRT group the exit is deep and the exchange rate is backed by a withdrawal queue, so a discount tends to close. For the BTC wrappers and the yield vaults, the exit is whatever redemption the issuer allows that week, which is what case 3 is about.
- USDe is the second largest group by debt at risk at 5%, 571M against 467M for rsETH, and 383M of it is one account. Whether that is a problem depends on one thing the oracle does not read, the USDe market price.

## Limits

The classification is by the adapter each reserve points to at the stated block. The holder lists come from Blockscout and cover the 50 largest aToken holders of each asset, balances and account data are read on chain at the block. The debt column is each account's total debt, not the part backed by the asset, which is why the collateral share column is there. The model moves one price at a time and holds everything else, so it says what one discount does to one group, not what a correlated move does to all of them. Market prices are DefiLlama's, labelled as such. The E-Mode categories in use were read from the Pool at the block, with their liquidation thresholds and collateral bitmaps. The reserve totals were read at block 26,426,783, eleven blocks before the holder data at block 26,426,794.

## Sources

- Aave V3 Pool `0x87870Bca3F3fD6335C3F4ce8392D69350B4fA4E2` (`getReservesList`, `getReserveData`, `getUserAccountData`, `getUserConfiguration`, `getUserEMode`, `getEModeCategoryCollateralConfig`, `getEModeCategoryCollateralBitmap`, `getEModeCategoryLabel`), AaveOracle `0x54586bE62E3c3580375aE3723C145253060Ca0C2` (`getAssetPrice`, `getSourceOfAsset`), each adapter's `description`.
- aToken balances and `totalSupply` for each reserve, holder lists from Blockscout.
- DefiLlama coins API for market prices.
