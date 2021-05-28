---
description: Trade function step-by-step
---

# Trade

When, in a given instant $$i$$, a trade is initiated and the contract will check the following parameters to proceed:

* Trade direction \(from token A to token B or the opposite\).
* The number of tokens the user wants to buy or sell \( `exactAInput` or`exactBoutput` \). The amounts considered in the trade will be called $$A_i$$ and $$B_i$$ , and they will be represented with the sign of the trade from the pool's point of view. If purchases options tokens, $$A_i$$ will be negative \(options tokens are leaving the pool\), and $$B_i$$will be positive \(stablecoins are entering the pool\).
* The total price slippage the user allows.
* Owner.

Once the contract gathered this information, it will start performing the following trade functions:

### 1. Calculate factors

### 1.1 Calculate Option Unit Theoretical Price

The contract will supply our BS implementation with the following data:

* the current spot price of the underlying asset
* time to maturity
* strike price
* risk-free rate
* last sigma \($$IV_{i-1}$$\)

And will get in return the updated option price \($$P_i)$$ based on the current market scenario.

![inside \_calculateNewABPrice function on optionAMMPool](../../../.gitbook/assets/screen-shot-2021-01-13-at-02.56.39.png)

### 1.2 Calculate total price based on the transaction amount

#### a\) Calculate pool amounts for each token:

$$\displaystyle poolAmountA = \min\left\{TB_{A};\frac{TB_{B}}{P_i}\right\}$$ 

$$\displaystyle poolAmountB = \min\left\{TB_{B};{TB_{A}}\cdot {P_i}\right\}$$ 

![\_getPoolAmounts at OptionAMMPool.sol](../../../.gitbook/assets/screen-shot-2021-01-13-at-03.00.16.png)

#### b\) Calculate product constant

$$k=poolAmountA*poolAmountB$$ 

#### c\) Calculate total transaction price, in terms of B

#### $$\displaystyle B_i=\frac{k}{(poolAmountA-tradeAmountA)}-poolAmountB$$ 

![](../../../.gitbook/assets/screen-shot-2021-01-13-at-03.06.11.png)

#### d\) Calculate new option unit price \(aka target price\)

The target price will be the input for the SigmaGuesser contract.

#### $$\displaystyle TargetPrice_i=\frac{poolAmountB-B_i}{poolAmountA+A_i}$$

![](../../../.gitbook/assets/screen-shot-2021-04-01-at-22.58.39.png)

{% hint style="info" %}
Note that on the contract level, for each of our 4 trade functions \(`exactAInput / exactAOutput / exactBInput / exactBOutput`\) the functions above are slightly different.
{% endhint %}

### 3. Find new IV correspondent to the new unit price

At this point, the pool's inventory changed, and it represents a new "virtual price." The IV corresponding to this new virtual price reflects market information of supply and demand for this option. 

So the AMM will input the new target price in the Black Scholes formula and find the new correspondent IV for this price. It uses a specific numerical method applied to Black Scholes. If you want to deep dive into how it works, explore [Find next IV.](https://app.gitbook.com/@pods-finance-1/s/pods_v1/~/drafts/-MakphibKovdOtH2XjQ4/options-amm-overview/optionamm/find-the-next-sigma)

The IV found will be stored in the contracts to later feed the Weighted Average with 25% weight. 

![newIV variable on any of the trade functions at OptionAMMPool](../../../.gitbook/assets/screen-shot-2021-01-13-at-03.09.59.png)

### 4. Updates

### 4.1 Update Total Balances

This step will update the pool's new total balances, considering the trade that just happened. 

$$TB_{A_{i}}=TB_{A_{i-1}} +A_i$$ 

$$TB_{B_{i}}=TB_{B_{i-1}} +B_i$$ 

{% hint style="info" %}
At the contract level,  $$A_{i}$$ are usually `exactAmountIIn or exactAmountAOut and` $$B_{i}$$ is `amountBIn / amountBOut` since we do not handle negatives numbers on solidity.
{% endhint %}

![](../../../.gitbook/assets/screen-shot-2021-01-13-at-02.46.54.png)

> Note that the trade function won't update the $$DB$$ nor the $$Fv$$ factor.

{% hint style="success" %}
Trade ✅
{% endhint %}

