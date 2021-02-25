# Pricing Options

## Pricing options

There are several models for pricing options in traditional finance, and the most widely known is [Black-Scholes](https://www.investopedia.com/terms/b/blackscholes.asp). Black-Scholes is a mathematical model for pricing an option contract, and this model assumes, among other things, that the asset's volatility remains constant over the option's life \(which is not applicable — especially in crypto\). The formula applies only to European options \(and to American calls on non-dividend paying assets\).

Other models are also commonly used, such as the [binomial model](https://www.investopedia.com/terms/b/binomialoptionpricing.asp) and the [trinomial model](https://www.investopedia.com/terms/t/trinomialoptionpricingmodel.asp).

{% embed url="https://www.youtube.com/watch?v=pr-u4LCFYEY" caption="Black-Scholes Formula by Khan Academy" %}

The mathematical notation of it is:

The Black-Scholes formula is as follows:

![](https://cdn-images-1.medium.com/max/800/1*kvlHeap4Kutcq9Ixspms5A.png)

where:

![](https://cdn-images-1.medium.com/max/800/1*LgTMcBuf3T9n8382cbLCzg.png)

## **Evaluating an option**

All methods are trying, to some extent, to calculate what is the probability the option will expire _in-the-money_ and, if it does, what that value would be at present. To calculate such theoretical price, authors incorporate factors like **underlying asset spot price,** **time to expiration**, **implied volatility**, **risk-free rate**, and strike price. Those factors, also known as greeks, have a different impact on the option price.

### The Main Greeks

**Delta**

Delta measures the impact a change in the _spot price_ of the _underlying asset_ has on the option price. For _put_ options, they represent a _negative_ number \(as the relationship between puts and spot price is conversely connected\) from -100 to 0. For _call_ options, they represent a _positive_ number from 0 to 100. For example, suppose that one _out-of-the-money_ option has a delta of 0.70 and another _in-the-money_ option has a delta of 0.99. A $1 increase in the underlying asset price will lead to a $0.70 increase in the first option and a $0.99 increase in the second option's respective premiums. 

Delta changes as the options become more profitable or _in-the-money_. In-the-money means that a _profit exists_ due to the option's strike price being more favorable to the underlying's price. As the option gets further in the money, delta approaches 1.00 on a call and -1.00 on puts.

**Gamma**

Gamma is a constant that represents the _rate of change of delta._  

**Theta**

Theta measures the impact _time_ has on the options price. The time impact is also called _time decay_, and it represents the erosion of an options price due to the passage of time. Theta is always accounting as a _negative_ factor since it can only move to one side, never stops, and since the options purchase, it starts diminishing the option price. In this sense, one could see theta as good for sellers and bad for buyers.  

**Vega**

Before talking about Vega, it's essential to understand what is _Implied Volatility \(IV\)_. Implied volatility \(IV\) or σ \(sigma\) captures the _market's sentiment_ about security. Almost as if it was responding to the question:

_"What is the likelihood that the price of the security will change in the future?"_

The higher this sentiment of uncertainty or wide volatility movements, the higher the asset's implied volatility. It is _not_ the historical volatility of the asset's spot price but rather the sentiment of the market about what range of movements could happen within time.

Vega tries to measure the changes in the expectations for future volatility. Its job is to respond to the question:

"_How much the option price should change if the market sentiment about asset implied volatility changes?_".  

Higher volatility makes options more expensive since the likelihood of the exercise event happens increases. Options sellers benefit from a fall in the implied volatility \(because the option they sold may not end up being exercise by expiration\). On the other hand, buyers don't benefit from it since it means that the likelihood of exercising is smaller. Therefore the premium they paid is "sank cost," or they will resell at a lower price in the secondary market, realizing a loss. 

Since Vega translates a change in sentiment around implied volatility, it can change even without changes in the underlying asset price. Fast movements on the underlying asset also move it, and it falls in importance and impact as it gets closer to the expiration. 

### **At expiration**

{% embed url="https://www.youtube.com/watch?v=PK5ProjUwfY" caption="ITM, OTM & ATM by Option Alpha" %}

The time to maturity of the option is also called the option's time value, is also an essential factor affecting pricing when using the Black-Scholes model.

The closer the option is from the expiration date, its intrinsic value tends to amplify. It means that if an option is _in-the-money_ closer to its expiration date, the more valuable it is as the probability of the option to be exercised and be profitable is high. The option price should reflect that impact and increase. The same logic happens when the option is _out-of-money_ close to its expiration date: the probability of exercise is low, which means that its value is close to worthlessness \(zero\).  

Find more details about this concept below:

![Source: https://www.investopedia.com/articles/optioninvestor/02/021302.asp](https://cdn-images-1.medium.com/max/800/1*5KrawYSrL08XEgTRV6lYgg.png)

At expiration, option prices are either _in-the-money_ or _out-of-the-money_. Most of the time, options end up _out-of-the-money_ and, therefore, at expiration, the options values zero.

{% hint style="success" %}
Now that you have the basics of regular options mechanics, it's time to learn about our implementation of \#DeFi options.
{% endhint %}

