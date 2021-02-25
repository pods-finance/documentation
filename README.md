---
description: "\U0001F44B Welcome to Pods' docs. This documentation aims to provide a high-level overview of the protocol and its existing components."
---

# Getting Started

## What is Pods?

Pods is a **decentralized non-custodial options protocol** that allows users to create calls and or puts and trade them in the **Options AMM**. Pods was implemented in a system of non-upgradable smart contracts in the Ethereum Blockchain.

DeFi options users confront three major pain points today:

* Users hold portfolios of volatile assets and want to hedge their risk to make sure they will keep their funds even in a black swan event. 
* Placing and managing options orders in decentralized order books can be costly, and the current solutions are not liquid enough to compensate for the gas cost.
* Providing options tokens as liquidity in Uniswap may translate into severe impermanent loss since it can only update the price based on a limited amount of inputs. That also means that users may not find reasonable market prices for buying or selling options in Uniswap.

Pods Protocol core components, Options Instrument and Options AMM, were built to address these pain points differently. 

Currently, users can participate:

* as sellers or buyers of either puts or calls in the Options Instrument and or 
* as liquidity providers in the Options AMM.

### **Options Instrument**

The [Options Protocol](https://app.gitbook.com/@pods-finance-1/s/teste/~/drafts/-MUJTFuPygKqO2rGEssD/options-protocol-overview/introduction) can `mint`, `unmint`, `exercise` and `withdraw` both **calls** and **puts.** The implementation requires the options to be fully collateralized. The options can be **European** or **American** and have a **physical** settlement, leaving no liquidation systems exposure.

### **Options AMM**

Our [options AMM](https://app.gitbook.com/@pods-finance-1/s/teste/options-amm-overview/introduction) is a **one-sided AMM** built to:

* Facilitate bootstrapping an options market that is initially illiquid by using an Automated Market Maker approach.
* Price options algorithmically using Black-Scholes and account for changes in factors such as `time to maturity` and `spot price`.
* Programmatically update the `Implied Volatility` of the model based on the pool's conditions.
* Keep options liquidity providers delta hedged during the provision period.
* Maintain the liquidity provider's exposure over time and distribute the pool's results equitably among participants.

Its implementation foresees an external price oracle's use to update the spot price of the underlying asset. 

### A Few Use Cases

* **Buy hedge and reduce hedge cost by earning fees in the AMM** Users that wish to buy hedge for their assets can buy put options and then provide them back to the pool as liquidity. It is possible because our AMM allows one-sided deposits and uses a concept of "exposure and position" throughout the pool lifecycle. By doing so, a user can earn fees with the options provided as liquidity even if the options get to expiration _out-of-the-money_. By the withdrawal, the user will get a combination of options tokens and stablecoins. The sum of both assets should represent the initial position, added the AMM fees and impermanent loss or gain, depending on how the pool performed over the period.
* **Mint options and earn AMM fees** By minting options and adding the options tokens as liquidity in the AMM, a user can keep its delta hedged and earn fees throughout the option lifecycle. By the time the user chooses to withdraw, it should receive a combination of options tokens and stablecoins. The sum of both assets should represent the initial position, added the AMM fees and impermanent loss or gain, depending on how the pool performed over the period.
* **Increase yield while selling options** Options sellers can use interest-bearing tokens as collateral, sell the options tokens to the AMM at the current price, and receive the premium. Therefore, it is possible to accumulate yields from aTokens \(currently integrated with Aave\) and premium for selling the option.

## Admin keys

‌Please be thoughtful about your funds while using the protocol in such an experimental phase.

The team currently holds admin powers on the protocol. See the details [here](https://app.gitbook.com/@pods-finance-1/s/teste/the-protocol/cap-and-admin-keys).

### This documentation 

> This documentation is a **work-in-progress**, and some parts **may be unfinished or pending updates**.
>
> If you find any errors or bugs or find something unclear or confusing, [let us know](https://discord.gg/yfgcHg)! We are looking forward to hearing your suggestions, questions, and feedback.

The following sections describe how options contracts work, how options work within our code, and how to interact with it. It also explains how our one-side exposure AMM works and how to interact with it.

Please join the \#development room in the [Pods community Discord server](https://discord.gg/yfgcHg); our team and community members look forward to helping you build on top of Pods.

{% hint style="success" %}
Now you've got a high level! Time to dig a little deeper.
{% endhint %}

