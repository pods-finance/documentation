---
description: Find below an exploration of different use cases on the platform.
---

# Use Cases

## **A few use cases** 

* **Buy hedge and reduce hedge cost by earning fees in the AMM**  Users that wish to buy hedge for their assets can buy put options and then provide them back to the pool as liquidity. It is possible because our AMM allows one-sided deposits and uses a concept of exposure instead of position throughout the pool life cycle. By doing so, the user may earn fees with the options provided even if the option gets to expiration _out-of-the-money_. The user can incur impermanent loss or gain, depending on the pool's circumstances during the liquidity provision period. 
* **Earn trading fees and be delta hedged** 

  By minting options and adding the options tokens as liquidity to the pool, a user is delta hedged and will gain trading activity fees. By the end of the period, the user can expect to receive back a position \(that could be a combination of options tokens and stablecoins\) that reflects their initial exposure, added trading fees, and impermanent gain or loss, depending on the pool's circumstances during the provision period.

* **Increase yield while selling options** Options sellers can use interest-bearing tokens as collateral and sell the options tokens directly in the AMM to receive the premium. That way, they accumulate yields from aTokens \(currently integrated with Aave\) and the premium for selling the option.

## **Bullish in the underlying asset**

There are multiple ways of expressing one's belief in a portfolio. For instance, if a user is bullish in a particular asset \(for example, ETH as the underlying asset\), they can **sell puts** or **buy calls** of the asset.  

### **Put Option Seller**

A put option represents the right to sell an asset in the future. When a user mints and sells a Put option, they sold that right to a buyer. They commit to buying the underlying asset from the option buyer at a specific price, which means that the option seller must be confident and comfortable about buying the underlying asset at the strike price. Options sellers expect to receive a premium to perform such activity.

#### Mint and Sell a Put Option

By minting and right away selling an option in the AMM, a user is closer to a conventional experience where they will immediately receive the premium at market price. Also, by directly selling the options tokens, the user immediately takes on the **position** "sold in Put options" and will uphold this position until the end of the expiration window. In case the seller decides to leave the position before expiration, the user has to buy the equivalent amount of options tokens in the market and "unmint" these options. Note that in the latter situation, the user is exposed to the difference in the option premium.

For example, if a user is minting one put option on ETH:aDAI, strike 500 December 21st, they will lock 500 aDAI as collateral and sell it in the AMM for the current market price - let's assume it's 5 DAI. If the user decides to leave the position after a while \(and before expiration\), it has to "buy back" options tokens in the market and unmint its position. The options tokens could be worth 2 DAI or 11 DAI, depending on the market conditions. 

If you want to learn more about price discovery, check [Pricing](https://docs.pods.finance/options-amm-overview/optionamm/pricing).

### Call Option Buyer

A call option represents the right to buy the underlying asset at the strike price in the future. Buying a call option allows the user to get exposed to an asset at a certain price level without necessarily holding that asset in the present moment. The user can buy call options tokens directly from the AMM and keep them in their wallet until expiration. If the user wants to leave the hedged position before expiration or wishes to profit over the option's price changes, they can resell the options on the AMM at market price. If the user held the options tokens until expiration, they could decide to exercise the options.

#### Buy Call Options 

Buying a call option gives the user a right to buy the underlying asset at the strike price. A user can buy a call directly from the series AMM and pay the premium immediately. By expiration, they can choose whether or not to exercise the option.

#### Buy Call Options and Provide Liquidity 

This possibility allows the options tokens to become interest-bearing assets within themselves.

A user that bought options can provide the options tokens as liquidity to the AMM and earn fees on the tokens and potentially reduce the cost of the options. By the end of the period, the user can expect to receive back a position \(that could be a combination of options tokens and stablecoins\) that reflects their initial exposure, added trading fees, and impermanent gain or loss, depending on the pool's circumstances during the provision period.

## Bearish in the underlying asset

If a user believes that the asset will or could go down in price, they can add different tools to its portfolio. Buying puts and selling calls.

### Put Option Buyer

#### Buy Put Option

Buying a put option gives the user the right to sell the underlying asset at the strike price. A user can buy a put directly from the AMM and pay the premium immediately. The user can hold its options tokens in their wallets until expiration. If the user decides, it can resell the options at market price back to the AMM. If the user chose to hold the tokens until expiration, it could determine whether it will exercise the option. If it decides to exercise the option, it has to send both underlying asset and options tokens to the contract to unlock the collateral.

#### Buy Put Option and Add Liquidity  

This possibility allows the options tokens to become interest-bearing assets within themselves. 

A user that bought options can provide the options tokens as liquidity to the AMM and earn fees on its tokens and potentially reduce the cost of their options.

By doing so, the user may earn fees with the options provided even if the option gets to expiration _out-of-the-money_. The user can incur impermanent loss or gain, depending on the pool's circumstances during the liquidity provision period.  

### Call Option Seller

To sell a call option, the user has to lock collateral \(in this case, the collateral is the underlying asset itself - for this reason, it works pretty much like a covered call\), mint the options tokens, and sell the options tokens in the AMM for the current premium. The collateral can be held in an interest-bearing representation \(like aETH\) and generate additional yield for the option seller. The users who mint a call option have to be comfortable selling the underlying asset at the strike price. Selling covered calls can increase the asset's "passive" yield in one's portfolio. 

#### Mint and Sell a Call Option

By minting and right away selling a call option in the AMM, a user is closer to a conventional experience where it will immediately receive the premium at market price. Also, by directly selling the options tokens to the AMM, the user immediately takes on the **position** "sold in call options" and will uphold this position until the end of the expiration window. If it decides to leave the position early, this user has to buy options tokens in the market and "unmint" his options. Note that in the ladder situation, the user is exposed to the difference in the option premium.

For example, if a user is minting one call option on aETH:DAI, strike 500 December 21st, they will lock 1 aETH as collateral and sell it in the AMM for the current market price. Let's assume it's 5 DAI. If the user decides to leave the position after a while \(and before expiration\), it has to "buy back" options tokens in the market and unmint its position. The options tokens could be worth 2 DAI or 11 DAI, depending on the market conditions. 

If you want to learn more about price discovery, check [Pricing](https://docs.pods.finance/options-amm-overview/optionamm/pricing). 

If exercised, the user will withdraw the yield generated during the period by the interest-bearing collateral and 500 DAI. If not exercised, the user will withdraw the aETH \(yield + principal\).

## Providing liquidity

The Pods Options AMM allows for single-sided liquidity provision. This property enables a user to add options tokens or stablecoins, or both. The AMM charges trading fees from traders and distributes the trading fee equally to the options tokens side and stablecoins side.

‌ By minting an option token and adding it to the pool, a user is delta hedged and has exposure to earning trading fees.

### Options Tokens

#### Mint Put Options tokens and Add Liquidity 

Alternatively, the user that chooses to add liquidity with the minted options tokens immediately gets **exposure** to the total option position. The AMM will track the initial exposure. By the time of the withdrawal, the user will receive a combination of options tokens and stable coins that should reflect the initial exposure. It means that the number of stablecoins received should be enough to "buy back" the lack of options tokens received by the time of the withdrawal. 

The user will earn AMM fees over time and is subjected to the pool's impermanent loss or gain. Our [fee](https://docs.pods.finance/options-amm-overview/optionamm/fees-updated) mechanism was designed to reduce this possibility, but it is essential to state that it is possible. 

#### Mint Call Options and Add Liquidity 

Alternatively, a user can mint a call option and add the minted tokens as liquidity in the AMM pool. This user immediately gets a delta hedged **exposure** to the total option position but **not necessarily is in the position.** The AMM will track the initial exposure. By the time of the withdrawal, the user will receive a combination of options tokens and stable coins that should reflect the initial exposure. It means that the number of stablecoins received should be enough to "buy back" the lack of options tokens received by the time of the withdrawal. 

The user will earn AMM fees over time and is subjected to the pool's impermanent loss or gain. Our [fee](https://docs.pods.finance/options-amm-overview/optionamm/fees-updated) mechanism was designed to reduce this possibility, but it is essential to state that it is possible.

### Stablecoins

Providing stablecoins or interest-bearing tokens as liquidity to an options pool enables users to earn trading fees. The AMM will track the user's initial exposure and return a position that should reflect the initial exposure by the end of the period. The user is subjected to impermanent gain or loss, depending on the pool's circumstances.

