# How old the prices Aave uses are, Ethereum Core

Read at three Ethereum blocks, 26,448,038 on 19 November 2026, 26,455,281 on 20 November and 26,475,919 on 23 November.

## Summary

At each block I followed the price of every reserve on Aave V3 Ethereum Core down to the Chainlink aggregators it reads, and measured how long before the block each of their answers had been written.

Two answers, USDC / USD and USDT / USD, price USDC, USDT, USDe, sUSDe and syrupUSDT. On 23 November those five reserves held 8.47B of the 22.47B supplied, 38%, and both answers were about 15 hours old. On 19 and 20 November, read near 09:00 UTC, they were about 18 hours old.

Over the twelve days to 22 November both feeds were written once a day, between 14:50 and 14:57 UTC, and at no other time. From one day to the next the USDC answer moved at most 0.010% and the USDT answer at most 0.052%.

## The two stablecoin feeds

USDC / USD read 0.99984000 on 23 November, written on 22 November at 14:54:47 UTC. The twelve answers from 11 to 22 November run from 0.99973126 to 0.99989601.

USDT / USD read 0.99854242, written on 22 November at 14:56:35 UTC. The twelve answers run from 0.99836000, written on 21 November, to 0.99932677.

USDe, sUSDe and syrupUSDT have no feed of their own in this path. Aave prices USDe at the USDT / USD answer, and sUSDe and syrupUSDT at that answer times their exchange rate. So they move only when the USDT / USD answer moves.

That share is growing. USDe supplied went from 0.64B on 19 November to 1.00B on 23 November. USDC supplied went from 2.72B to 4.16B over the same days. Both nodes return the same figures. I have not looked at who supplied them.

## The rest of the reserves

The ETH / USD and BTC / USD answers were under one hour old at all three blocks, and they carry WETH, weETH, wstETH, rsETH, osETH, cbBTC and LBTC. WBTC adds a WBTC / BTC answer that was between 8.4 and 11.4 hours old. PYUSD / USD and USDG / USD were between 9.8 and 12.9 hours old, USDS / USD between 9.9 and 11.1, and LDO / ETH between 9.1 and 21.4.

## What this does not say

An old answer is not a wrong answer. A feed that is written once a day while its asset sits near one dollar is doing what it is set to do, and every answer above was within 0.17% of one dollar. What I am still trying to work out is how far USDT would have to move before its feed is written outside the daily slot, and how many of the accounts that borrow against USDe and sUSDe would be below health factor 1 at that point.

## Limits

Three blocks, two near 09:00 UTC and one near 06:00 UTC, so they catch the stablecoin feeds in the second half of their day. The round history covers twelve days of two feeds. I did not read the off chain settings of any feed, only what the contracts return.

## Method

Every figure is a contract read at the block, checked on two nodes, ethereum-rpc.publicnode.com and eth-mainnet.public.blastapi.io. The answers, timestamps and supplied amounts matched exactly on both at each block.

- Aave V3 Pool `0x87870Bca…A4E2` (`getReservesList`, `getReserveData`) and AaveOracle `0x54586bE6…a0C2` (`getSourceOfAsset`, `getAssetPrice`), each aToken (`totalSupply`).
- Each price source followed through its adapter slots (`BASE_TO_USD_AGGREGATOR`, `ASSET_TO_USD_AGGREGATOR`, `ASSET_TO_PEG`, `PEG_TO_BASE`) down to the aggregators, at most four levels.
- Each aggregator (`latestRoundData`, `description`), age taken as block timestamp minus `updatedAt`.
- USDC / USD `0xEa674bBC…496B` and USDT / USD `0x3E7d1eAB…32D` (`getRoundData` for the twelve rounds before the 23 November block).
