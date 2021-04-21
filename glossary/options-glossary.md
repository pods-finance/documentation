---
description: >-
  Find below a glossary of the commonly used terminology used throughout Pods'
  material.
---

# Glossary of Terms

If we're missing any, please let us know on our [Discord channel](https://discord.gg/Qf7utym), and we'll add it to the list. 

| Term | Description |
| :--- | :--- |
| **Underlying asset** | The object asset of the contract, the asset on which the buyer wants to buy rights. |
| **Strike price**  | The agreed price in which the buyer will be able to sell or buy the underlying asset to the seller at any moment until expiration. |
| **Strike asset** | The asset in which the strike price is fixed and the asset will execute the physical settlement. |
| **Expiration date**  | The contract is valid until the expiration date. |
| **Put** | A put option describes the right to **sell** the underlying asset at the strike price. |
| **Call** | A call option describes the right to **buy** the underlying asset at the strike price. |
| **Collateral asset** | The collateral asset is the strike asset and has to be locked in the contract to mint PodOptions. |
| **Premium** | The premium is the option's price. It is the amount of DAI that the buyer has to pay for the seller to purchase an option. |
| **PodOptions** | It is our primitive. An ERC20 token that represents the option's contract rules. |
| **PodPut** | The token represents the buyer's right to **sell** the underlying asset at the strike price at any moment until expiration. |
| **PodCall** | The token represents the buyer's right to **buy** the underlying asset at the strike price at any moment until expiration. |
| **Option Token** | PodOptions are the Pods Protocol primitive. They can be either PodCalls or PodPuts. |
| **AMM** | Automated Market Maker. |
| **Options AMM** | Our options specific AMM. |
| **Black Scholes** | It is an options pricing formula widely used in traditional finance.  |
| **Liquidity Provider** | It is the user that provides funds as liquidity to the AMM in exchange for trading fees. |
| **Seller** | It is the user that locks collateral in the contract and mint options tokens.  |
| **Buyer** | It is the user that buys options from the AMM. |
| **Implied Volatility** | Implied Volatility is the market's forecast of a likely movement in the underlying asset spot price. In our model, the IV derives from the pool's activities and imbalance.  |
| **Time to expiration** | Amount of time left to the option expiration date. |
| **Impermanent loss** | Potential loss a liquidity provider could get from providing liquidity to the AMM instead of holding the assets. |
| **Impermanent gain** | Potential gain a liquidity provider could have from providing liquidity to the AMM instead of holding the assets. |

