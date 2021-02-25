---
description: The general properties of the Options AMM
---

# Options AMM

An options AMM implies a pool with USDC \(or aUSDC\) and options tokens \(PodPuts or PodCalls\). The options tokens can be bought or sold to the AMM pool, and their price is known as premium. The Options AMM contract algorithmically calculates the premium. The pools created in the options AMM will stop the trading functionality once the option reached the expiration date. However, it is still possible to withdraw funds that were provided as liquidity.

The Options AMM has the following properties:

* **Price discovery   
  The AMM algorithmically calculates premium**

  The algorithm accounts for the current spot price of the underlying asset \(leveraging ChainLink's oracle\), the time until expiration, the implied volatility \(calculated based on supply and the demand for options tokens observed in the pool\).

* **Single-sided liquidity provision**  It is possible to add liquidity on one side of the pool. The pool will track the user's initial exposure and, by the time the user removes liquidity, the withdrawal position should reflect the initial exposure within a new distribution of assets. The new position will be composed of a new asset distribution \(stable coins and option tokens\), impermanent gain or loss \(depending on the pool's return\), and AMM fees. 
* **Inventory imbalance and price changes**  If there is no inventory imbalance \(meaning canceling trades happened in the pool\), price changes due to price calculations should not take any value from liquidity providers.
* **Fair distribution on pool returns among liquidity providers**  Pool returns should be distributed correctly across time and liquidity providers.

\*\*\*\*

General derivatives price formulations use one or more data from the market \(like the spot price of the underlying asset\) to calculate a [derivative price](https://app.gitbook.com/@pods-finance-1/s/teste/~/drafts/-MN6HaWpBmzbOVxj8TTa/understand-options/pricing-options). Our model's price discovery mechanism uses external factors, combined with computed properties \(such as time to expiration and risk-free rate\) and internal factors \(such as the Implied volatility\), to calculate the current premium according to Black Scholes. For more details on pricing, check [Pricing](https://app.gitbook.com/@pods-finance-1/s/teste/v/master/options-amm-overview/optionamm/pricing).

