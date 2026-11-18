# How four lending venues price wstETH

Read 16 November 2026 at Ethereum block 26,426,794.

## Summary

Aave V3, Spark, Compound V3 and Morpho Blue account for 2.39M wstETH at this block, 64% of the 3.75M in existence.

All four start from the same number, the wstETH to stETH exchange rate that Lido's contracts report, 1.249306 at the block. What differs is what that rate is multiplied by. On Aave and Spark it is an ETH / USD price. On Compound it is an ETH / USD price, or a constant 1 in the WETH market. On Morpho it depends on the market. The two largest WETH markets read the exchange rate and nothing else. The six largest that lend USDT, USDC or EURCV read a feed that also multiplies by Chainlink's STETH / ETH price, which stood at 0.999456.

## Aave V3

Core prices wstETH with an adapter described as Capped wstETH / stETH(ETH) / USD. It takes the ratio from the stETH contract and multiplies it by an ETH / USD aggregator that read 1,795.31, which gives 2,242.89. The adapter caps the growth of the ratio at 8.80% a year and reported the cap as not active. 846,930 wstETH were supplied, 1.90B at that price. The liquidation threshold is 82%, or 95% in the ETH correlated E-Mode. Aave's Lido market uses the same adapter and had 45,694 wstETH supplied.

## Spark

Spark's price source for wstETH has no description. It exposes two addresses, the stETH contract and an ETH / USD source described as an aggregate of Chronicle, Chainlink and RedStone, which read 1,798.07. That figure times the same ratio gives 2,246.35. 1,221,531 wstETH were supplied, 2.74B at that price. The liquidation threshold is 84%, or 93% in the ETH E-Mode.

## Compound V3

The USDC, USDT, USDS and Institutional USDC markets share one feed, described as wstETH / USD CAPO SVR Price Feed. It multiplies the ratio reported by the wstETH contract by an ETH / USD aggregator that read 1,794.55, which gives 2,241.94. Ratio growth is limited to 4.04% a year and the cap was not active. The WETH market uses a wstETH / ETH CAPO feed whose base is a constant 1, so it returns the ratio itself. Across the five markets 88,987 wstETH were posted as collateral, 37,937 of it in the USDC market and 28,897 in the WETH market. The liquidation collateral factors run from 85% to 93%.

## Morpho Blue

The Morpho contract held 185,915 wstETH. Each market names its own oracle, so I read the eight markets that Morpho's API lists with the most wstETH collateral.

The two WETH markets, at 96.5% and 94.5% LLTV, share one oracle with a single feed, described as wstETH/stETH exchange rate. It returned 1.249306 WETH per wstETH. 28,100 and 7,420 WETH were borrowed in them.

The other six lend USDT, USDC or EURCV at 86% LLTV. All six use the same base feed, described as Custom price feed for wstETH / ETH. That feed names Chainlink's STETH / ETH aggregator as its source and returned 1.248626, which is the ratio times the 0.999456 that aggregator showed. The largest of the six, wstETH / USDT, had 96.1M USDT borrowed and priced wstETH at 2,241.89 USDT.

## Where the market price of stETH enters

The STETH / ETH feed was 0.054% under 1, and its last update was about four and a half hours old. The six Morpho markets that read it priced wstETH 0.054% below what the exchange rate alone gives. On Aave, Spark, Compound and the two Morpho WETH markets that feed is not among the contracts the price path reads.

The ETH / USD source differs too. With the same ratio, Aave, Spark and Compound returned 2,242.89, 2,246.35 and 2,241.94 for one wstETH, 0.20% between the highest and the lowest.

## Limits

I read eight of the 71 Morpho markets that list wstETH as collateral, so the other 63 are not covered. The 2.39M adds what is supplied on Aave and Spark, what is posted as collateral on Compound and what the Morpho contract holds. I did not read what sits behind each ETH / USD aggregator beyond its description.

## Method

Every figure is a contract read at the block, checked on two nodes. Each price was rebuilt from its inputs and matched to the last digit.

- wstETH `0x7f39C581…2Ca0` (`stEthPerToken`, `totalSupply`, `balanceOf`), stETH `0xae7ab965…fE84` (`getPooledEthByShares`).
- Aave V3 Pool `0x87870Bca…A4E2`, Lido market Pool `0x4e033931…58B1` and AaveOracle `0x54586bE6…a0C2` (`getReserveData`, `getAssetPrice`, `getSourceOfAsset`), adapter `0xe1d97bf6…61ef` (`RATIO_PROVIDER`, `BASE_TO_USD_AGGREGATOR`, `isCapped`, `getMaxYearlyGrowthRatePercent`).
- Spark Pool `0xC13e21B6…E987` and oracle `0x8105f69d…cfd9`, source `0xe98d51fa…ca39` (`ethSource`, `steth`).
- Compound V3 markets (`getAssetInfoByAddress`, `totalsCollateral`, `getPrice`), feeds `0xa2699232…9acf` and `0x91359ce8…4ee9` (`ratioProvider`, `assetToBaseAggregator`, `maxYearlyRatioGrowthPercent`).
- Morpho Blue `0xBBBBBbbB…FFCb` (`idToMarketParams`, `market`), each market oracle (`price` and its four feed slots), feeds `0x4f67e4d9…8e79` (`stETHtoETHPriceFeed`) and `0x905b7dab…1421`.
- Chainlink STETH / ETH `0x86392dC1…2812` (`latestRoundData`).
