---
description: Options AMM overview
---

# Overview

### General Purpose AMM

A general-purpose AMM, such as Uniswap v1 and v2, is one of the essential building blocks in DeFi. Its model is capable of combining price discovery and bootstrap in any market in the world. It allows a market for even the most illiquid assets to exist and grow.

Its design carefully combines incentives to encourage different players to participate as sellers, buyers, and liquidity providers. The liquidity provider is a player that performs a vital role in the system. It feels like an abstraction of the traditional market maker. That is because it doesn't require constant book management and orders filled but allows users to trade against their funds. Some may argue that those are not the same users nor perform the same business. But at the end of the day, both players \(liquidity providers in Uniswap and market makers in traditional finance\) bootstrap a market.

‌Currently, the liquidity provider has to add equal amounts in both sides of the pool as liquidity. In return, it expects to collect all the trading fees that happened in the pool and get their principal amount invested back.

Some users in DeFi have been reading or understanding the activity of providing liquidity as a "passive income investment" where one provides assets, holds the dollar amount of the funds, and receives a positive sum of fees for the initial investment. Although this might be true for pools of stable assets \(DAI:USDC, for instance\), it is not entirely correct for pools with volatile assets \(USDC:ETH, for example\) in specific scenarios. There is a phenomenon known as **impermanent loss**.

Impermanent loss \(IL\) is the expression we use to refer to the negative return of adding liquidity to Uniswap on a volatile pair when compared to just holding both assets. It describes a situation where the liquidity provider's profitability that was seeking passive income is negatively impacted. Impermanent loss is inexistent or small in the scenario where the price scenario between the tokens added is back to the same ratio they were when the liquidity was initially added. But, in case the price conditions are different from what initially was, the withdrawal can be lower than the total amount provided compared to the scenario of just holding the assets.

That means that impermanent loss is only realized if the liquidity provider \(LP\) removes the liquidity in a price scenario different from the one initially used. If it doesn't remove it and wait for a more opportunistic moment to withdraw, the loss won't happen \(_impermanent_ loss\). Compared to traditional finance, one could think of this as a "selling a perpetual straddle" position. Perpetual because there is no expiration date on this position, and the LP can hold it for as much time as they want.

![Short Straddle payoff.](https://cdn-images-1.medium.com/max/800/0*LIlRuZUjl5TWzG-C.jpg)

It means that the LP will profit from the fees during the period and not suffer from impermanent loss if, even with volatile markets, it removes liquidity at the same price level that it had when it entered. Some could argue that they expect that the trading fees will cover their losses. It can happen only to a certain extent, depending on factors such as how far the price is at the moment of the withdraw, how liquid, and how many trades happened in the pool during the liquidity provision.

Although this model may have a perfect fit with non-volatile assets and highly voluminous trading pools, it may not be accurate for all kinds of assets.

It's fair to understand that the most profitable combination for liquidity providers is a pool where the assets are **not volatile**, and there are **many trades per day**. The latest variable is explained by the fact that the higher amount of transactions happening per day, the closest the price of the assets within the pool will be from the spot price in the market.

### Options Pools

In a DeFi option \(specifically Pods options\), a user has to lock 100% in collateral and **mint** the option token. The option token represents the right for the buyer and the obligation for the seller. But, since the seller locked the collateral upfront, there is not much to be enforced; the smart contract holds the commitment.

After minting the option, a user can choose to sell them or provide liquidity to the pool. As Uniswap is a decentralized protocol with a pool of any pair of assets started by anyone, users could sell the options tokens directly in the Uniswap pool for each series. By doing so, the user is:

a\) assuming that the options market for that specific options series is the most liquid it can be and, therefore, the price of the AMM is correct by pure arbitrage forces and represents a fair, updated price.

‌ or

‌ b\) arbitrating the pool price if the pool is mispricing the options and there is an opportunity to close the gap.

DeFi is only starting, and DeFi options are also just beginning. This means that not necessarily the DeFi options market can correct Uniswap's price at all times. It means that if a pool stays for a long time without trades, there will be a significant arbitrage opportunity. While this may be good for traders, it may not be as good for the liquidity providers.

> But how would someone know that an option price is mispriced?

**Learn more about pricing options** [**here**](https://docs.pods.finance/understand-options/pricing-options)**.** 

All methods are trying, to some extent, to calculate what is the probability the option will expire _in-the-money_ and, if it does, what that value would be at present. To calculate such theoretical price, authors incorporate factors like **underlying asset spot price,** **time to expiration**, **implied volatility**, **risk-free rate**, and strike price. Those factors, also known as greeks, have a different impact on the option price.  

### Options trading in General Purpose AMMs

Each trader treats the greeks and formulas as it seems fit. But most of them have some way of calculating the option price that can account for all the factors that impact an option price. Traders would likely find ways to open arbitrage opportunities, but liquidity providers ultimately could result in losses for allocating their capital to potential mispriced trades.

‌ For this reason, using a model like Uniswap for pricing options may not be the best alternative for liquidity providers.

We decided to create an Options AMM capable of calculating the options price using outside vectors \(such as spot price of the underlying asset\) and internal vector \(such as implied volatility\) to address our own needs as users and liquidity providers.

Some of the characteristics developed in the Options AMM are:

* Allows for single-sided liquidity provision 
* Calculates the options price using Black Scholes
* Prices options using Black Scholes by updating factors such as time to maturity, spot price, and implied volatility change over time. For more details on pricing, check [Pricing](https://docs.pods.finance/understand-options/pricing-options). 

Like the assets it holds \(options\), the AMM pools are created and expire within the option's lifetime. After an option enters the exercise window, users can only withdraw funds from the pool. After users withdraw funds from the pool, they may use their options tokens to exercise an option or, in the case of a seller, it may wait until the end of the exercise window to withdraw the collateral \(or underlying asset, in case of an exercised option\) locked in the contract.

