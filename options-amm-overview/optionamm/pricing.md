---
description: >-
  Pods Options AMM implements Black Scholes to price the options
  algorithmically.
---

# Pricing

There are multiple models for options pricing. The most widely used model is called [Black Scholes](https://www.investopedia.com/terms/b/blackscholes.asp). It calculates European options' theoretical value using the current underlying asset price, option's strike price, expected risk-free interest rate, time to expiration, and expected volatility. 

Although the Options Instrument allows American options series to be created, it is not recommended to implement them using the AMM current pricing model. Bear in mind that the options are represented in ERC20 tokens and can be sold anywhere \(not necessarily in the options AMM\) so one can still put together an American option and use it elsewhere.

### Assumptions

The AMM's implementation of the model makes certain assumptions:

* European Options \(exercised only during the expiration window\)
* No dividends are paid out during the option lifetime
* Markets are efficient \(moves cannot be predicted, and arbitrage opportunities will be taken instantly\)
* The risk-free rate is accounted as 0% at this point
* Changes in supply and demand for tokens reflected in the pool are expressed as changes to the implied volatility in the model for future pricing.

An option price translates the exercise's possibility based on probability and market sentiment towards the amplitude of change the underlying asset can have. The model describes what should be the correct price of an option today that expresses the correct probability of this event happening in the future so that, if the option is correctly price, there is no space for arbitrage.

The model assumes the underlying asset follows a [lognormal](https://www.investopedia.com/articles/investing/102014/lognormal-and-normal-distribution.asp) distribution since asset prices cannot be negative \(they are bounded by zero\). The lognormal distribution is asymmetrical and creates a right-skewed curve. The [skewness](https://www.investopedia.com/terms/s/skewness.asp) represents the degree of distortion from the symmetrical bell curve in a probability distribution. Data on both sides of the skew are also referred to as "tail."

The Black Scholes model allows for discovering a new price based on current and past values. 

### Original Formula

The put option pricing formula is calculated as follows:

$$c$$ = Call option price

$$p$$ = Put option price

$$S$$= Current Underlying Asset Spot Price

$$K$$ = Option's Strike price

$$r$$ = Risk-Free Rate

$$t$$ = Time to Maturity

$$N$$ = A normal distribution

![](../../.gitbook/assets/screen-shot-2020-11-30-at-2.29.42-am.png)

The factor  ![](../../.gitbook/assets/screen-shot-2020-11-30-at-2.33.19-am.png) discounts the strike price to present value at the risk-free rate. In our implementation, the risk-free rate is 0%, and for this reason the factor $$e^r=1$$.This means that we are not impacting the time value of the money into the strike price. The understanding that drives this decision is that there is no consensus among a risk-free rate for DeFi at this point.

The factor ![](../../.gitbook/assets/screen-shot-2020-11-30-at-2.41.17-am.png) calculates the probability that the underlying asset will be at or above the strike price by expiration.

The factor ![](../../.gitbook/assets/screen-shot-2020-11-30-at-2.42.52-am.png) calculates the future underlying asset price if the asset is above the strike by expiration.

### AMM's Price discovery

#### Initial Price

To create a pool, the creator has to define the initial price of the pool. The user is also defining the implied volatility of the pool opening moment \($$IV_i$$\). 

One could think of the IV as a factor that is guessed by the contract to fulfill the following: 

Applying the implemented formula, what IV generates this option price \(the initial price defined by the option creator\), considering the current market conditions? 

{% hint style="warning" %}
The contracts accept the first IV that generates a price range of {-0.1;+0.1} to avoid infinite calculations.
{% endhint %}

Once this initial IV is guessed, the contract will store this as the pool's initial level in terms of supply and demand.

####  Price updates

With the first price calculated by the contract, all following prices will run the same price update process. In a given instant $$i$$ a trade can be initiated, the contract will check the following parameters to proceed:

* Trade direction \(from token A to token B or the opposite\)
* How many tokens A will deliver \(or how many tokens B wants to receive\) `exactAInput` `exactBoutput`
* What total slippage accepted
* Owner

The contract will do the following to calculate the unit price for the option:

#### 1. Update underlying asset spot price

We're leveraging Chainlink to update the underlying asset spot price.   

#### 2. Calculate new time to maturity

The contract will calculate how long it will take until the expiration date. 

#### 3. Check latest sigma \($$IV_{i-1}$$\)

The previous \(or initial\) calculated price saved an $$IV_i$$ factor of that time. This IV factor now is $$IV_{i-1}$$since a period has passed. This factor will be used as an input to calculate the new option price. Using the last period's IV means that the price always has a 1-period delay to its IV. 

#### 4. Input strike price

Check the options parameter and use it as an input.

#### 5. Input risk-free rate

As mentioned above, the risk-free rate is fixed at 0% at this point.

The contract will gather this information to feed the Black Scholes contract. Considering these conditions,  it will:

#### 1. Calculate the unit price for the option using the implemented BS formula

#### 2. Calculate total price for trade amount

For more details, check [Trade](functions/trade.md).

#### 3. Calculate new price based on inventory balance after the trade

For more details, check [Trade](functions/trade.md).

#### 4. Guess new sigma \($$IV_i$$\) and store it in the contract for the next trade.

After the trade, the pool's inventory changed, and it represents a new "virtual price." This price is used to guess the new sigma after the trade. In the next trade, the sigma calculated now will be used to calculate a new price then. If you want to deep dive into this topic, check our Find next sigma section.

### Pricing Flow

See below a diagram that explains the pricing flow when there is a trade happening. 

![Pricing flow during trades.](../../.gitbook/assets/pricing-flow.png)

{% file src="../../.gitbook/assets/pricing-flow \(1\).pdf" caption="Pricing flow in PDF" %}

