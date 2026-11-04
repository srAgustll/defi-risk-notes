# PT-reUSD liquidations on Morpho, where the collateral went

Liquidations of 25 August 2026, Ethereum blocks 25,829,868 to 25,829,936. Read from Morpho's API and the transaction receipts on 23 October and 2 November 2026.

## Summary

On 25 August two Morpho markets that lend USDC and USDT against PT-reUSD had 33 liquidations in under 14 minutes. 36.1M was repaid, 38.6M PT was seized and no bad debt was left.

The usual way to think about a liquidation is that the seized collateral gets sold somewhere, so the depth of that venue sets how much can be cleared. That day 1.5% of the seized PT went into the Pendle pool. 96.1% never left Morpho. One liquidator repaid the debt with a flash loan, put the seized PT back in the same markets as collateral and borrowed against it again. The exposure stayed in the market and changed owner.

## The markets

| Market id | Lends | Collateral | Oracle | LLTV |
|---|---|---|---|---|
| `0x1e9d614631a7df0ec07fb05b2c8cb2491575fd1a63a33bf187a6afb295a4fc64` | USDC | PT-reUSD-10DEC2026 | `0x217d6DdCdB95112C51657f6270e8C079cfdB51f0` | 91.5% |
| `0x6acd18854e43f93f5a90446004c35c72a68af1ad99f08cb247782ce6457914ca` | USDT | PT-reUSD-10DEC2026 | same oracle | 91.5% |

The PT token is `0xecFAfDc7741323a945A163eD068B5a3C43483957`. Market parameters read with `idToMarketParams` on Morpho Blue `0xBBBBBbbBBb9cC5e90e3b3Af64bdAF62C37EEFFCb`.

## How much room a liquidator has

Morpho pays the liquidator an incentive that depends only on the LLTV. In the contract it is the smaller of 1.15 and 1 / (1 − 0.3 × (1 − LLTV)). At an LLTV of 91.5% that gives 1.0262, so a liquidator can sell the seized collateral up to about 2.55% below the oracle price and still break even before gas.

Sigma Labs calls this budget σ* in [The Liquidator's Frontier](https://forum.morpho.org/t/2301/15). Their formula and the contract give the same number.

## What happened

| Item | Value |
|---|---|
| Liquidations | 33 (16 in the USDC market, 17 in the USDT market) |
| First and last | 04:37:47 and 04:51:23 UTC |
| Debt repaid | 36,144,246 |
| PT seized | 38,638,598 |
| Bad debt | 0 |
| Oracle price implied by each liquidation | from 0.9657 down to 0.9442 USD per PT |

The implied oracle price is repaid × incentive / seized for each liquidation. It fell about 2.2% in 14 minutes.

## Where the seized PT went

Each liquidation transaction was traced to the first place the seized PT was sent.

| Destination | PT | Share |
|---|---|---|
| Back into the same Morpho markets as collateral, one liquidator, 13 liquidations | 37,144,276 | 96.1% |
| Sold into the Pendle pool `0x13285bcbc27f92b47b4edb99d744c07b48c977c0`, 16 transactions | 583,912 | 1.5% |
| Kept in the liquidator's wallet `0x681B072b7555C4C57a12eEC8E90a2914EE733816`, 2 liquidations | 902,541 | 2.3% |
| Sent through a router, 1 liquidation | 7,870 | 0.02% |

## The liquidator that did not sell

Liquidator contract `0x51a453d677396F62fbb1dff9925a205fa96fAB5e` did all 13 of the large ones. One example, [tx 0xe1bfa2ef…](https://etherscan.io/tx/0xe1bfa2ef3a94fc74a72caec588ce36b59a58d43d735d8e2fdea89fdb0053eb76) at block 25,829,871, step by step.

1. Flash loan of 101,233,884 USDC from Morpho.
2. Repay 11,010,095 USDC of a borrower's debt and seize 11,710,808 PT.
3. Supply the 11,710,808 PT to the same market as collateral, on behalf of `0xd67d8f6b6ba0a97f043305f6b6f5a8ed1a247f44`.
4. Borrow 9,942,416 USDC against it.
5. Repay the flash loan, with about 1.07M of the liquidator's own USDC covering the gap.

The new position starts at about 88% LTV (9.94M against 11.71M PT at the 0.9648 oracle price), close to the 91.5% LLTV.

Across the 13 transactions the liquidator repaid 34.76M, supplied 37.14M PT back as collateral and borrowed 31.39M again in the same two markets (30.89M USDC and 0.50M USDT, per Morpho's API). Its own capital was about 3.4M.

## The sales into the Pendle pool

The smaller liquidators sold the PT into the Pendle pool in the same transaction. Example, [tx 0x47a4c2cc…](https://etherscan.io/tx/0x47a4c2cc75912409268e27f9c26be1a4e69d57335e75b6622f40720bea83372c) at block 25,829,878.

1. Flash loan of 109,615 USDC from Uniswap V3.
2. Repay 108,978 USDC and seize 116,183 PT.
3. Sell the PT into the Pendle pool for 99,958 SY-reUSD and unwrap it to reUSD.
4. Swap the reUSD back to stablecoins and repay the flash loan.

reUSD is not a dollar. It accrues yield and was worth 1.0965 USD at the time (DefiLlama). The 116,183 PT therefore sold for about 0.943 USD each, against an implied oracle price of 0.9625 for that liquidation, a discount of 2.0%.

Over the 16 sales the discount to each liquidation's implied oracle price ranged from 0.0% to 2.1%, with a weighted average of 1.17%. Every sale fit inside the 2.55% budget.

## What this says about exit liquidity

On this day the pool was not the binding constraint, because almost nothing was sold into it. Clearing capacity came from two other places, the USDC and USDT available to borrow in the same Morpho markets, and the liquidator's own capital for the gap between the debt repaid and the new borrowing.

That has two consequences for anyone sizing liquidation risk on PT collateral.

- Pool depth alone understates how much can be cleared, as long as the lending market has liquidity to re-borrow.
- The exposure to the PT does not fall after a liquidation like this one. It moves to a new owner at a higher LTV, inside the same market.

What happens when the lending market itself is short of liquidity, and re-borrowing is not available, is the case this day did not test.

## Limits

The destination of each PT is the first transfer after the seizure inside the liquidation transaction. What the liquidator did with the new position afterwards is not traced here. The reUSD price is a single DefiLlama reading at 04:51 UTC and the liquidations ran over 14 minutes.

## Sources

- Morpho API, liquidations for both market ids on 25 August 2026.
- Transaction receipts for all 33 liquidation transactions (Transfer, Liquidate, SupplyCollateral and Borrow events).
- `idToMarketParams` on Morpho Blue for both markets.
- Morpho Blue source, `ConstantsLib.sol` (LIQUIDATION_CURSOR 0.3, MAX_LIQUIDATION_INCENTIVE_FACTOR 1.15) and the liquidate function in `Morpho.sol`.
- DefiLlama historical price for reUSD `0x5086bf358635B81D8C47C66d1C8b9E567Db70c72`.
- Sigma Labs, [The Liquidator's Frontier](https://forum.morpho.org/t/2301/15).
