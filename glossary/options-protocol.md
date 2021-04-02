---
description: "\U0001F64B\U0001F3FD‍♂️No-dumb-questions page."
---

# FAQ

## What are options? <a id="eef2"></a>

An option is an agreement between two parties: a seller and a buyer. The seller writes the contract terms and sells them to the buyer for a premium. That means the seller now has an obligation, and the buyer has a right \(but not an obligation\) to exercise the contract terms upon expiration.

## What are American options? <a id="6e46"></a>

There are two types of options: European and American. European options are a point-in-time instrument since they allow the user to exercise **only** at the expiration date. On the contrary, American options are a continuous-time instrument since they will enable the user to exercise the contract **anytime before the expiration.**

## What are **European** options? <a id="6e46"></a>

There are two types of options: European and American. European options are a point-in-time instrument since they allow the user to exercise **only** at the expiration date. Right now, Pods' implementation is ready for both European and American options. It is important to note that European options are commonly Black Scholes \(which our code is currently implemented to use\), but it is not so well suited for American options. Even though it is possible to create an American option series, it is not recommended. If the community wishes, we can work on having a better-suited pricing mechanism for American options in the future.

## **What types of options are there?** <a id="ec54"></a>

There are two basic types of options: puts and calls. A put represents the right to **sell** the underlying asset at the strike price. On the other hand, a call conveys the right to **buy** an asset at the strike price.

## What is a Pod? <a id="e318"></a>

A PodOptions \(PodCall and PodPut\) is our primitive. It is an ERC-20 that represents a right granted to its holder. It means that whoever minted an option token issued an obligation to comply with the contract terms at expiration \(considering European options - which is our primary case\). This obligation is sold for a price \(premium\) to a buyer. In other words, the buyer purchases the right to exercise the contract terms at expiration.

> PodOptions are minted with a 1:1 parity to its underlying asset.

## What is PodPut? <a id="e318"></a>

A PodPut is an ERC20 that represents a put option token.

## What is PodCall? <a id="e318"></a>

A PodCall is an ERC20 that represents a call option token.

## What is an option series, and how does it work? <a id="5977"></a>

An **option series** is defined by a unique combination of an underlying asset, strike asset, strike price, and maturity, and each option is fungible inside its series.

## What is the strike price? <a id="2f53"></a>

The strike price is commonly used to designate the price at which the underlying asset will be sold or bought in the future.

## How is the options premium calculated? <a id="b95b"></a>

In traditional finance, option premiums are rather hard to calculate, and there are several pricing models out there. The most widely used is Black-Scholes, and it takes into account factors like intrinsic value, time to expiration, and underlying asset volatility. Our protocol implemented the Black Scholes formula for pricing the options algorithmically, and it considers a 1-period delay for updating the implied volatility. The implied volatility is calculated based on the supply and demand observed in a pool. For more details about pricing, check [Pricing](https://app.gitbook.com/@pods-finance-1/s/teste/~/drafts/-MNRC2ZsPjFTrrRMLycS/options-amm-overview/optionamm/pricing).

## What asset can I offer as collateral for writing a put? <a id="4d0f"></a>

If you are minting a put option, you have to lock DAI. If you're minting a call option, you have to lock ETH. Our contracts are also ready for interest-bearing tokens, so instead of DAI and ETH, a user could lock aDAI and aETH, respectively.

## If I have PodOption and I want to exercise it, what should I do? <a id="cc08"></a>

Enter the website, connect your MetaMask wallet, and go to [Exercise](https://pods.finance/hedger/exercise). There you should hit "buy" and then hit "Exercise". You'll be able to execute your position, giving the amount of PodOptions you own.

## If I sold PodOption and expiration has not reached yet, how can I exit my position? <a id="f9ef"></a>

An option seller can unmint its previously minted options by buying options tokens in the market and going to the unmint process. By the end of the process, the seller should be able to withdraw its collateral. In that case, you'd be subjected to the current premium the market is trading. That means you could either benefit or get a loss with this unmint process.

## What is the put buyer's potential PnL \(Profit and Loss\)? <a id="d8dc"></a>

_Quick recap:_ a buyer is someone who owns PodPuts and, therefore, it holds the right to sell the underlying asset for the strike price in the future but not after the expiration.

The PnL of an option is a function of strike price and the current market price \(spot price\) of the underlying asset. So, in the case of an ETH:DAI put option, the PnL depends on the current market price of ETH, in terms of DAI.

Imagine that the buyer bought 1 PodPut for a 3 DAI premium. If ETH is trading at 90 DAI \(every ETH costs 100 DAI\) and the strike price was 150, we say that this option is _in-the-money_, and the buyer should execute it. In this case, the buyer's profit would be 57 DAI.

$$
Strike Price - Spot price - Premium = Buyer's Payoff
$$

$$
150-90-3 = 57
$$

If ETH's price is trading above the strike price, there's no reason for the buyer to execute the option \(we say that this option is _out-of-the-money_\). This means that the payoff PnL is a loss of the premium paid.

Observing the buyer's payoff, one can see that the buyer can only profit in case the price of the underlying asset **falls below the strike price**. This means that by buying PodPut, one can protect against downside volatility.

## What is the put option seller's potential PnL? <a id="1501"></a>

_Quick recap:_ a seller is someone who minted and sold PodPuts in the past and now holds an obligation towards the buyer. This obligation is guaranteed by the collateral the seller provided in the contract. That means that whenever the buyer wants to exercise the PodPut, the seller's collateral will be used to buy PodOption from the option holder at the strike price.

The PnL of an option is a function of a strike price and the current market price \(spot price\) of the underlying asset. So, in our case, the PnL depends on the current market price of ETH in terms of DAI.

Imagine that the seller sold 1 unit of PodPut at a premium of 3 USDC and had precisely 150 DAI locked in as collateral, considering a strike price of 150 DAI pert unit of ETH. If ETH is trading at 100 DAI, we say that this option is _in-the-money_, and the seller is forced to commit to its contract by repurchasing the PodPut + 1 unit of ETH for 150 DAI. If the seller were to compare the trade it was "forced" to do with the trade she could have done in the market, she'd notice representing a 47 DAI loss. Notice that the buyer's profit is always equal to the loss of the seller.

Observing the seller's payoff, one can see that it only profits if the underlying asset does not fall below the strike price.

This example does not consider the potential gain with the collateral held in aTokens during the option lifetime.

## Why can't I set a price for the put I'm writing? <a id="2b36"></a>

An option token is an ERC20 that can be sold wherever the user wants to \(another DEX, an order book, P2P\). To make it easier for users to find liquidity, we developed an options-specific AMM that calculates the option price algorithmically based on Black Scholes. For this reason, if a user wants to interact with the protocol as a whole, it will be subjected to the AMM's pricing conditions.

## What happens if I have collateral locked and expiration is reached? <a id="9260"></a>

If you minted PodOptions and locked the equivalent amount of DAI our ETH, your option can be exercised by the buyer until the expiration is reached. If the expiration was reached and the option was not exercised, you can unlock the contract's collateral after the exercise window closes.

## **Why do we need a token for writing options on-chain?** <a id="195b"></a>

Using PodOptions to allow writing and trading options on-chain, we are also saying that the option contract is tokenized. That is an important feature to increase the system's overall liquidity, making it easier to integrate with other DEX and protocols since it's a generic ERC20 token.

## Have more questions?

Reach out to us on our channels:

* [Twitter](https://twitter.com/PodsFinance)
* [Discord](https://discord.gg/Qf7utym)

