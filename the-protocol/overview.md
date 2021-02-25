---
description: >-
  The overall protocol is a combination of the Options Instrument and the
  Options AMM.
---

# Overview

## How it works

The combination between the Options Instrument and the Options AMM is what comprises Pods Protocol.

![Pods Protocol overview](../.gitbook/assets/protocol-overview.png)

### **Options Instrument**

The options instrument describes how a user can create and manage an option token.

Users can `mint`, `unmint`, `exercise`, and `withdraw` both **calls** and **puts.** The implementation requires the options to be fully collateralized. The options can be **European** or **American\*** and have a **physical** settlement, leaving no liquidation systems exposure. The general functions are mint options tokens, unmint, exercise, and withdraw funds after expiration.

{% hint style="warning" %}
\*Although the code is ready to receive either American or European options, it is **not recommended** to implement American options using the current pricing model applied on the Options AMM. Find more about this in the Pricing section. 
{% endhint %}

The Options Instrument only defines the functions that will impact the creation of an option and its exercise. The trading and pricing facilities are held in the Options AMM. 

* Options tokens are standard ERC20 tokens and can potentially be sold on another DEX or a P2P basis. 
* Anyone can create new options series at any time. 

### **Options AMM**

Pricing and trading options tokens happen in the [options AMM](https://app.gitbook.com/@pods-finance-1/s/teste/options-amm-overview/introduction). For that to happen, the Options AMM:

* Enables single-sided liquidity provision.
* Algorithmically prices the options using Black Scholes.
* Update factors such as time to maturity and spot price.
* Programmatically updates the Implied Volatility change over time based on the conditions of the pool. For more details, check [Pricing](https://app.gitbook.com/@pods-finance-1/s/teste/~/drafts/-MUJTd3NADF5p4jYrmxE/options-amm-overview/optionamm/pricing). 

Like the assets it holds \(options\), the AMM pools are created and expire within the option's lifetime. After an option enters the exercise window, users can only withdraw funds from the pool. After users withdraw funds from the pool, they may use their options tokens to exercise an option or, in the case of a seller, it may wait until the end of the exercise window to withdraw the collateral \(or underlying asset, in case of an exercised option\) locked in the contract. 

### **Overall System Flow**

1. **Create**
   1. **Create an option series.** Anyone can create a new option series using our non-upgradable contracts from the Options Instrument.
      1. **Mint and unmint options.** Option sellers can lock collateral and _mint_ options to provide them as liquidity to the pool in its opening moment or to sell the options tokens in another DEX. If the user chooses to leave the position before the expiration, they can do so by using the _unmint_ function.
   2. **Create an option pool in the AMM.** Anyone can create a new option pool using the Options AMM, just like starting new pools in Uniswap.
      1. **Add Liquidity to the pool.** To create a new pool, add liquidity on both sides \(options tokens and stablecoins\). After the first moment, the one-sided liquidity provision is unlocked, and anyone can add liquidity users can deposit either both assets or only one asset. 
2. **Mint and unmint options.** Option sellers can lock collateral to _mint_ options and either provide them as liquidity to the pool or sell them directly to the pool. If the user chooses to leave the position before the expiration, it can do so using the _unmint_ function.
3. **Trade options.** Option sellers and buyers can sell or buy \(to and from the pool\) until the expiration date. When the expiration date is reached, it is no longer possible to trade options in the AMM. Instead, it is only possible for liquidity providers to remove their liquidity.
4. **Exercise.** Options buyers have a specific and pre-determined exercise window to exercise their options.
5. **Remove liquidity from the pool.** When the exercise window starts, the only possible action is to withdraw funds from the pool. 
6. **Withdraw collateral.** After the expiration window closes, options sellers can withdraw the equivalent amount of funds they are eligible for. That could be a combination of the underlying asset and strike asset if the option was exercised.

Different reasons engage different users in using the protocol. Let's take a close look at the [Ecosystem Participants](https://app.gitbook.com/@pods-finance-1/s/teste/~/drafts/-MUJTd3NADF5p4jYrmxE/the-protocol/ecosystem-participants) and the [Use Cases](https://app.gitbook.com/@pods-finance-1/s/teste/~/drafts/-MUJTd3NADF5p4jYrmxE/the-protocol/use-cases) next.

