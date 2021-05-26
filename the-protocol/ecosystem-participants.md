---
description: >-
  Multiple use-cases allow for various users with different motives to
  participate.
---

# Ecosystem Participants

Pods ecosystem comprises four types of users: options sellers, options buyers, liquidity providers, and developers. There may be overlap between users \(for instance, an option seller that is a liquidity provider or an option buyer also a liquidity provider\). 

* Liquidity providers are incentivized to contribute ERC-20 tokens as liquidity to the pool, especially to earn fees on the trading activity that happened within the pool.
* Options sellers can mint, sell or add liquidity, unmint, and withdraw positions of calls and puts. 
* Options buyers can buy, hold, add tokens as liquidity to the pool and exercise the options tokens.
* Developers can integrate directly with Pods smart contracts to leverage new experiences with options to users.

### Options Sellers

Options sellers could be:

* **Put option sellers:** either is interested in buying the underlying asset at a strike price anyway and could use a discount \(considering premium received and yield from interest-bearing collateral during the option lifetime\) or believes that the price won't reach the strike price. It won't be exercised, so it wants to make an additional yield in its interest-bearing token. 
* **Call option sellers:** already holds a long position in the underlying asset and wants to increase the portfolio's yield by selling options. 

### Options Buyers

Option buyers could be:

* **Put option buyers:** buying hedge for its long position in the portfolio or betting the asset price will go down and wants to profit at that moment \(by purchasing the asset at market price and selling it with the option exercise with a profit\). 
* **Call option buyers:** users that want to get exposure to a specific asset at a certain price level. 
* **Volatility traders:** users that typically price the options themselves and continuously compare the market price with their estimated fair price and act accordantly by buying or selling options tokens when it makes sense to them. Most of the time, they do not have the intention of exercising the option.

### Liquidity Providers

By allowing single-sided liquidity provision, it is possible to have many different types of liquidity providers:

* **Minted a put option and provided liquidity:** by minting options tokens and providing them as liquidity in the AMM, a user is interested in generating an additional yield on its funds based on the AMM fees. The AMM will track its initial exposure and will accrue trading fees. By the time the user decides to withdraw, they will receive a combination of options tokens and stablecoin representing virtually the same initial exposure the user had in the beginning. Meaning, if the user wanted all their options tokens back, the amount of stablecoin received is enough to cover the purchase amount of the current "missing" options tokens at the updated premium price of the options tokens. The user may incur impermanent loss or gain, depending on the pool conditions during the period of the provision. 
* **Minted a call option and provided liquidity:** call option seller wants to monetize its long position by minting an option and providing it as liquidity to the AMM to earn trading fees.
* **Bought a put option and provided liquidity:** user wants to reduce the price of the premium paid and maybe recover funds \(earned from the fees\) from an option that would end up _out-of-the-money_. 
* **Bought a call option and provided liquidity:** user wants to reduce the price of the premium paid and maybe recover funds \(earned from the fees\) from an option that would end up _out-of-the-money_. 
* **Stablecoin liquidity provider:** Like other AMMs, the user wants to earn passive income from trading fees by providing liquidity to a pool. The user may incur impermanent loss or gain, depending on the pool conditions during the period of the provision. 



### **Developers**

There are many ways Pods can be used in the broader Ethereum ecosystem. One example is:

* The open-source, accessible nature of the protocols developed at Pods means countless UX experiments and front-ends were built to offer access to Pods Protocol.



