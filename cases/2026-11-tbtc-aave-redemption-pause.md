# tBTC on Aave while redemption is paused, the price Aave sees and the exit a liquidator has

Read 6 November 2026 at Ethereum blocks 26,353,936 (exit quotes) and 26,353,950 (positions), with two earlier reads on 3 and 5 November for the exit.

## Summary

On 31 October Threshold paused tBTC minting and redemption, and Aave froze the tBTC reserve on Ethereum Core the same morning ([LlamaRisk, governance.aave.com/t/25965](https://governance.aave.com/t/25965)). A frozen reserve keeps its liquidation threshold, so the 1,627.6 tBTC inside it still counts as collateral, and the accounts holding most of it carry about 85M of debt.

Aave prices tBTC with a BTC / USD feed. A tBTC discount to BTC would not reach any health factor, but a fall in BTC would, and 96% of the debt against the largest tBTC positions is in stablecoins. If those positions are liquidated while redemption is closed, the liquidator receives tBTC and can only exit through secondary liquidity. On chain that exit absorbs about 135 tBTC before the liquidation bonus is used up. Three days earlier it absorbed about 215, and a 30% fall in BTC would have put about the same amount into the first round of liquidations. Today the same fall puts about 245 tBTC into the first round, nearly twice what the exit takes.

## The reserve

| Item | Value |
|---|---|
| Reserve | tBTC `0x18084fbA666a33d37592fA2633fD49a74DD93a88`, Aave V3 Ethereum Core |
| Status | frozen, not paused |
| LTV / liquidation threshold / bonus | 0% / 78% / 7.5% |
| Protocol fee on the bonus | 10%, so the liquidator keeps 6.75% |
| Supplied | 1,627.6 tBTC, 38.9% of the 4,185.2 tBTC on Ethereum |
| Price source | `0xb41e773f…8b0a`, Chainlink BTC / USD |

Since the freeze the reserve has shrunk. It held 1,744.7 tBTC on 3 November, 1,693.6 on 5 November and 1,627.6 today. Withdrawals are allowed on a frozen reserve, and one of the 50 largest holders took its 50.7 tBTC out between 5 and 6 November.

## The price Aave sees

| Source | Value |
|---|---|
| Aave oracle for tBTC (BTC / USD) | 75,384 |
| Chainlink TBTC / USD `0x8350b7De…4297`, last update 5 Nov 15:53 UTC | 75,561 |
| Curve tBTC/WBTC, 1 tBTC sold | 0.9984 WBTC |

At small size tBTC still trades at BTC. The Chainlink TBTC / USD feed exists and is live, but Aave does not use it. Before listing, BGD described two options, a market feed with CAPO or BTC / USD trusting the BTC reserves and avoiding DEX manipulation, and the second is what is on chain.

So the question for this reserve is not whether a tBTC discount triggers liquidations. It does not. The question is what a liquidator does with the tBTC once something else does.

## Who holds it

The 50 largest aTBTC holders hold 1,435.5 tBTC (88.2% of the reserve). 49 of them use it as collateral and borrow, with 85.1M of debt between them.

| Debt asset | Amount |
|---|---|
| USDC | 28.5M |
| USDT | 24.5M |
| DAI | 21.7M |
| GHO | 6.1M |
| WETH | 3.3M |
| EURC | 1.0M |

None of these accounts is in E-Mode. 28 of the 49 hold tBTC as their only collateral, so their health factor moves one to one with BTC / USD. The lowest is 1.13.

## How much tBTC a fall in BTC would release

Model. Every BTC collateral falls by the same amount, ETH and stablecoins stay where they are, debt is unchanged. Aave lets a liquidator repay half the debt while the health factor is between 0.95 and 1, and all of it below 0.95. The first round is what liquidators can take in one call per account.

| BTC fall | BTC / USD | Accounts below HF 1 | Their debt | tBTC in those accounts | tBTC in the first round |
|---|---|---|---|---|---|
| 10% | 67,845 | 0 | 0 | 0 | 0 |
| 15% | 64,076 | 1 | 1.0M | 19.5 | 8.5 |
| 20% | 60,307 | 2 | 1.4M | 27.0 | 21.4 |
| 25% | 56,538 | 3 | 1.9M | 39.4 | 31.6 |
| 30% | 52,769 | 11 | 20.0M | 452.1 | 244.7 |
| 40% | 45,230 | 15 | 29.0M | 599.2 | 582.5 |

## The exit

tBTC sold for WBTC through ParaSwap at block 26,353,949.

| tBTC sold | WBTC received | Discount to WBTC |
|---|---|---|
| 50 | 49.7 | 0.6% |
| 100 | 96.6 | 3.5% |
| 150 | 138.9 | 7.4% |
| 200 | 175.1 | 12.5% |
| 250 | 206.0 | 17.6% |
| 300 | 228.6 | 23.8% |
| 500 | 276.8 | 44.7% |
| 1,000 | 286.1 | 71.4% |
| 1,744 | 280.8 | 83.9% |

Most of the depth is the crvUSD/tBTC Curve pool `0x4f52c3a81e33521e5a9a47fd9d3be475d2279c2e`, with 19.7M crvUSD against 215.2 tBTC. The three tBTC/WBTC pools in the route hold 72.7 WBTC against 174.4 tBTC together.

A liquidator keeps 6.75% of the collateral it seizes. Selling on chain stops paying when the discount passes 1 − 1 / 1.0675, or 6.3%, which on this curve is around 135 tBTC.

## The exit over three reads

| Read | Block | BTC / USD at Aave | WBTC in the three tBTC/WBTC pools | tBTC in them | 200 tBTC sold, discount | tBTC the exit absorbs at 6.3% |
|---|---|---|---|---|---|---|
| 3 Nov | 26,332,529 | 76,757 | 119.5 | 96.4 | 4.9% | about 215 |
| 5 Nov | 26,346,888 | 80,405 | 86.0 | 157.3 | 11.2% | about 150 |
| 6 Nov | 26,353,949 | 75,384 | 72.7 | 174.4 | 12.5% | about 135 |

In three days the WBTC pools gave up 47 WBTC and took in 78 tBTC. The crvUSD pool went from 194 to 215 tBTC. Whoever is leaving tBTC is leaving it in the pools a liquidator would need, and each tBTC that arrives there is one less the exit can take.

## What this says

- Up to a 25% fall in BTC, the tBTC that becomes liquidatable is small, 32 tBTC in the first round, and the on chain exit can take it.
- At 30% the first round is about 245 tBTC against an exit of about 135. On 3 November the two numbers matched. Since then BTC has moved both ways and the positions with it, but the exit has only shrunk.
- A liquidator past the break even point either holds the tBTC until redemption reopens, or does what one liquidator did with PT-reUSD on Morpho on 25 August and keeps the collateral inside the lending market ([case 2](2026-08-pt-reusd-liquidations-morpho.md)). On Aave the frozen reserve does not accept new tBTC supply, so that second route is closed.
- The exit is the moving part. The positions change with BTC, which is public and slow. The exit changes with whoever sells tBTC into the WBTC pools, and this week that has been faster than the positions.

## Limits

The exit curve is a quote at one block. In a 30% BTC fall the pools would be repriced and arbitraged, and their depth in BTC terms could be different. The position model holds every non BTC price still and does not include accounts outside the 50 largest (192.1 tBTC). Each fall is applied in one step. A gradual fall would liquidate the same accounts earlier and in smaller pieces, but nothing would refill the exit while redemption is closed. The holder list comes from Blockscout, balances and account data are read on chain at the stated block.

## Sources

- Aave V3 Pool `0x87870Bca3F3fD6335C3F4ce8392D69350B4fA4E2` (`getReserveData`, `getUserAccountData`, `getUserConfiguration`, `getUserEMode`), AaveOracle `0x54586bE62E3c3580375aE3723C145253060Ca0C2` (`getAssetPrice`, `getSourceOfAsset`), the data provider's `getLiquidationProtocolFee`.
- aTBTC `0x10ac93971cdb1f5c778144084242374473c350da` balances, variable debt token balances for each borrowed reserve.
- Chainlink TBTC / USD `0x8350b7De6a6a2C1368E7D4Bd968190e13E354297`, `latestRoundData`.
- Curve tBTC/WBTC `0xb7ecb2aa52aa64a717180e030241bc75cd946726` (`get_dy`, `balances`), crvUSD/tBTC `0x4f52c3a81e33521e5a9a47fd9d3be475d2279c2e` (`balances`), tBTC/WBTC pools `0x73a38006d23517a1d383c88929b2014f8835b38b` and `0xdbac78be00503d10ae0074e5e5873a61fc56647c` (`balances`).
- ParaSwap price API.
- LlamaRisk, [tBTC Mint & Redeem Pause, Aave's Response](https://governance.aave.com/t/25965).
