---
description: >-
  There could be countless combinations of events. Let's see their impact in the
  pool with a few examples.
---

# Applied Math

##  Examples

The examples below aim to highlight a few properties that the AMM pools currently hold and their effects on the pool and liquidity providers.

For simplicity, consider the following information about the pool and the option:

| input name | Description Pool |
| :--- | :--- |
| asset pair | ETH:DAI |
| option type | Put |
| exercise type |  European |
| strike price | $400 |
| spot price \(Chainlink\) | $500 |
| expiration | 31 Dec 2020 |
| current day | 21 Nov 2020 |
| $$Fv_i$$  | 1 |

{% hint style="warning" %}
Please consider that the function "add" in this context stands for adding liquidity for the first time in a pool or when the pool has no imbalance, meaning, in both cases $$Fv_i=1$$ . Check [`add liquidity`](https://app.gitbook.com/@pods-finance-1/s/teste/~/drafts/-MNRwe7cZHIbL-jPcZGu/v/master/options-amm-overview/optionamm/functions/add-liquidity) for further details. 
{% endhint %}

## APR

#### Add, Price Moves, Remove

This example explores the scenario where a user adds liquidity for the first time. The option price was updated mostly based on external factors \(time passed and time to maturity changed or spot price of the underlying asset changed\). There are no trades during the period the user kept liquidity in the pool, and the user removes liquidity without losing value on its initial deposit.

In this scenario, we'll see that the user will withdraw the same amount of assets it deposited initially, even if the price changed from $$P_{i-1}$$ to $$P_i$$ .

#### Example Information

The event `adding liquidity` will happen in the instant  $$i= 0$$ with the following information:

* $$A_{du}=100$$ options
* $$B_{du}= 205$$ DAI
* Owner = John 

### Adding liquidity

#### 1\) Calculate Factors

####  1.1\) Calculate new price \($$P_i$$\)

The price will be calculated by the BS contract and will return a unit price, $$P_i$$. Consider that the unit price calculated was 2 DAI per option for this example.

$$P_i=2$$

#### 1.2\) Calculate pool's value factor

No inventory imbalance means $$Fv_i=1$$ :

$$\displaystyle Fv_i= \frac{(TB(A)_{i-1}\cdot P_i+TB(B)_{i-1})}{(DB(A)_{i-1} \cdot P_i+DB(B)_{i-1})}$$ 

$$TB(A)_{i-1}=DB(A)_{i-1} $$ 

$$TB(B)_{i-1}=DB(B)_{i-1}$$ 

#### 2\) Updates

#### 2.1\) Update User Balance

$$UB(A)_u=A_i$$ 

$$100 = A_i$$ 

$$UB(B)_u=B_i$$ 

$$205=B_i$$ 

#### 2.2\) Update Pool Factor in the moment of the user's deposit

$$UB(F)_u=1$$ 

#### 2.3\) Update total balances of the pool

$$TB(A)_i=TB(A)_i-_1 +A_{du}$$ 

$$TB(A)_i=0+100 = 100$$ 

$$TB(B)_i=TB(B)_{i-1} +B_{du}$$ 

$$TB(B)_i=0 +205=205$$ 

#### 2.4\) Update deamortized balance of the pool for each factor

$$\displaystyle DB(A)_i=DB(A)_{i-1} + \frac{A_{du}}{Fv_i}$$ 

$$\displaystyle DB(A)_i=0+\frac{100}{1}=100$$ 

$$\displaystyle DB(B)_i=DB(B)_{i-1} +\frac{B_{du}}{Fv_i}$$ 

$$\displaystyle DB(B)_i=0+\frac{205}{1}=205$$ 

{% hint style="success" %}
Add liquidity ✅
{% endhint %}

### Remove liquidity

Assuming that there were no trades, the price moved, and the user wanted to remove liquidity.

#### 1\) Calculate new price 

The price will be calculated by the BS contract and will return a unit price, $$P_i$$. Consider that the unit price calculated was 3 DAI per option for this example.

$$P_i=3$$ 

#### 2\) Calculate the Pool's opening factor

Since $$TB(A)_i= DB(A)_i$$ and $$TB(B)_i= DB(B)_i$$, $$Fv_{i+1}=1$$ 

$$\displaystyle Fv_i= \frac{(TB(A)_{i-1}\cdot P_i+TB(B)_{i-1})}{(DB(A)_{i-1} \cdot P_i+DB(B)_{i-1})}= 1$$

#### 2.2\) Calculate multipliers

$$\displaystyle mAA_i= \frac{min(Fv_i\cdot DB(A)_{i-1};TB(A)_{i-1})}{DB(A)_{i-1}}=1$$ 

$$mAA_i=1$$ 

$$\displaystyle mBB_i= \frac{min(Fv_i\cdot DB(B)_{i-1};TB(B)_{i-1})}{DB(B)_{i-1}} =1$$

$$mBB_i=1$$

#### 2.3\) Calculate withdraw amount of each token

The following will happen for a 100% withdrawal in both tokens:

$$\displaystyle A_i=-[mAA_i\cdot r_a\cdot \frac{UB(A)u}{Fv_{du}}+mBA_i\cdot r_b\cdot \frac {UB(B)u}{Fv_{du}}]$$ 

$$-UB(A)_u =-A_i$$ 

$$-UB(A)_u=-100$$ 

$$\displaystyle B_i=-[mBB_i\cdot r_b\cdot \frac{UB(B)u}{Fv_{du}} + mAB_i \cdot r_a\cdot \frac {UB(A)_u}{Fv_{du}}] $$

$$-UB(B)_u =-B_i$$ 

$$-UB(B)_u=-205$$ 

Therefore, the users in this scenario will withdraw the same amount of tokens in the same proportion they provided initially. 

There are further steps on the remove liquidity function, but after calculating the withdrawal amount for each token, it is possible to see that the user won't lose value if there is a price change with no trades. 

{% hint style="info" %}
Price moves without trade won't impact liquidity providers' withdrawal value amount either the proportion of assets.
{% endhint %}

## ATR

#### Add, Trade, Remove 

This example explores the scenario where a user adds liquidity for the first time, and the option price was updated. There are trades followed by immediate withdraw of funds.

In this scenario, we'll see that the user will withdraw a different amount of assets from what it had deposited originally. Still, its withdrawal value is not impacted since $$P_i = P_{i-1}$$ .

#### Example Information

The event `adding liquidity` will happen in the instant  $$i= 0$$ with the following information:

* $$A_{du}=100$$ options
* $$B_{du}= 205$$ DAI
* Owner = John 

### Adding liquidity

#### 1\) Calculate Factors

####  1.1\) Calculate new price \($$P_i$$\)

The price will be calculated by the BS contract and will return a unit price, $$P_i$$. Consider that the unit price calculated was 2 DAI per option for this example.

$$P_i=2$$

#### 1.2\) Calculate pool's value factor

No inventory imbalance means $$Fv_i=1$$ :

$$\displaystyle Fv_i= \frac{(TB(A)_{i-1}\cdot P_i+TB(B)_{i-1})}{(DB(A)_{i-1} \cdot P_i+DB(B)_{i-1})}= 1$$ 

$$TB(A)_{i-1}=DB(A)_{i-1} $$ 

$$TB(B)_{i-1}=DB(B)_{i-1}$$ 

#### 2\) Updates

#### 2.1\) Update User Balance

$$UB(A)_u=A_i$$ 

$$100 = A_i$$ 

$$UB(B)_u=B_i$$ 

$$205=B_i$$ 

#### 2.2\) Update Pool Factor in the moment of the user's deposit

$$UB(F)_u=1$$ 

#### 2.3\) Update total balances of the pool

$$TB(A)_i=TB(A)_{i-1} +A_{du}$$ 

$$TB(A)_i=0+100 = 100$$ 

$$TB(B)_i=TB(B)_{i-1} +B_{du}$$ 

$$TB(B)_i=0 +205=205$$ 

#### 2.4\) Update deamortized balance of the pool for each factor

$$\displaystyle DB(A)_i=DB(A)_{i-1} +\frac{A_{du}}{Fv_i}$$ 

$$\displaystyle DB(A)_i=0+\frac{100}{1}=100$$ 

$$\displaystyle DB(B)_i=DB(B)_{i-1} +\frac{B_{du}}{Fv_i}$$ 

$$\displaystyle DB(B)_i=0+\frac{205}{1}=205$$ 

{% hint style="success" %}
Add liquidity ✅
{% endhint %}

#### Example Information

The event `trade` will happen in the instant  $$i= 1$$ given the following information:

* $$A_{du}=-2$$ options \(negative to show that the options will leave the contract\)
* Trade direction = user wants to receive exact amount of token A \(trade function will follow the `exactAOutput` setup\)
* Max price slippage 20%
* Owner = Gui 

### Trade

#### 1\) Calculate Factors

####  1.1\) Calculate new price \($$P_{i+1}$$\)

The price will be calculated by the BS contract and will return a unit price, $$P_i$$. Consider that the unit price calculated was 4 DAI per option for this example.

$$P_i=4$$

#### 1.2\) Calculate total price based on transaction amount

#### a\) Calculate pool amounts for each token:

$$\displaystyle poolAmountA = min(TB(A);\frac{TB(B)}{P_i})$$ 

$$\displaystyle poolAmountA=min(100;\frac{205}{4})$$ 

$$poolAmount A=51.25$$ 

$$poolAmountB=min(TB(B);TB(A)\cdot P_i)$$ 

$$poolAmountB=min(205;100\cdot 4)$$ 

$$poolAmountB=205$$ 

#### b\) Calculate product constant

$$k=poolAmountA\cdot poolAmountB$$ 

$$k=51.25\cdot 205= 10,506.25$$ 

#### c\) Calculate total transaction price, in terms of token B

#### $$\displaystyle B_i=\frac{k}{(poolAmountA-tradeAmountA)}-poolAmountB$$ 

$$\displaystyle B_i= \frac{10,506.25}{(51.25-2)}-205 $$ 

$$B_i=213.32-205= 8.32$$ 

#### 2\) Updates

#### 2.1\) Update Total Balances

$$TB(A)_i=TB(A)_i-_1 +A_{du}$$ 

$$TB(A)_i=100+ (-2)=98$$

$$TB(B)_i=TB(B)_i-_1 +B_{du}$$ 

$$TB(B)_i=205+8.32= 213.32$$ 

{% hint style="success" %}
Trade  ✅
{% endhint %}

### Remove liquidity

Assuming that John decides to remove the total liquidity immediately after the trade, considering that there were no changes in the price after the trade and removing liquidity events.

$$r_a=1$$ and $$r_b=1$$ 

#### 1\) Calculate new price 

The price will be calculated by the BS contract and will return a unit price, $$P_i$$. Consider that the unit price calculated was 4 DAI per option, equal to the previous period. 

$$P_i=4$$ 

#### 2\) Calculate the Pool's opening factor

Since there was a trade, the factors TB\(A\) and TB\(B\) are no longer equal to DB\(a\) and DB\(B\). Therefore, the pool's opening factor will be different from 1.

$$\displaystyle Fv_i= \frac{(TB(A)_{i-1}\cdot P_i+TB(B)_{i-1})}{(DB(A)_{i-1} \cdot P_i+DB(B)_{i-1})}$$

$$Fv_i=\frac{(98 \cdot 4+213.32)}{(100\cdot 4+205)}$$ 

$$Fv_i= 605.32/605=1.00052893$$ 

#### 2.2\) Calculate multipliers

$$\displaystyle mAA_i= \frac{min(Fv_i\cdot DB(A)_{i-1};TB(A)_{i-1})}{DB(A)_{i-1}}$$ 

$$mAA_i=min(100.0528;0.98)=0.98$$ 

$$\displaystyle mBB_i= \frac{min(Fv_i\cdot DB(B)_{i-1};TB(B)_{i-1})}{DB(B)_{i-1}}$$

$$mBB_i=min(205.1082;1.0405)=1.0405$$

$$\displaystyle mAB_i= \frac{TB(B)_{i-1}-mBB_i\cdot DB(B)_{i-1}}{DB(A)_{i-1}}$$ 

$$\displaystyle mAB_i=\frac{213.32-(1.0405\cdot 205)}{100}=0.000175$$ 

$$\displaystyle mBA_i= \frac{TB(A)_{i-1}-mAA_i\cdot DB(A)_{i-1}}{DB(B)_{i-1}}$$ 

$$\displaystyle mBA_i=\frac{98-(0.98*100)}{205}=0$$ 

#### 2.3\) Calculate withdrawal amount of each token

The following will happen considering a withdrawal of 100% of the initial liquidity provided on both tokens:

$$\displaystyle A_i=-[mAA_i\cdot r_a\cdot \frac{UB(A)u}{Fv_{du}}+mBA_i\cdot r_b\cdot \frac {UB(B)u}{Fv_{du}}]$$ 

$$A_i=-[0.98\cdot 1\cdot (\frac{100}{1})+0]=-98$$

$$\displaystyle B_i=-[mBB_i\cdot r_b\cdot \frac{UB(B)u}{Fv_{du}} + mAB_i \cdot r_a\cdot \frac {UB(A)_u}{Fv_{du}}] $$

$$B_i=-[1.0405\cdot1\cdot (\frac{205}{1})+0.000175\cdot 1\cdot (\frac{100}{1})]=-213.32$$ 

Therefore, in this scenario, the user had an impermanent gain expressed in the amount of token B in the withdrawal.

There are further steps on the remove liquidity function, but after calculating the withdrawal amount for each token, it is possible to verify the property exposed.

{% hint style="info" %}
Trade followed by withdrawal with no price move from the trade moment doesn't incur different value to be withdrawn but different proportions between assets, reflecting the trade.
{% endhint %}



## ATPR

#### Add, Trade, Price moves, Removes

This example explores the scenario where a user adds liquidity for the first time, the option price was updated, there are trades in the pool, e price moved again, and the user withdraws funds.

In this scenario, we'll see that the user will withdraw a different amount of assets from what it had deposited originally. They may reflect an impermanent loss or gain, the trade position, and the latest price movements. 

#### Example Information

The event `adding liquidity` will happen in the instant  $$i= 0$$ with the following information:

* $$A_{du}=100$$ options
* $$B_{du}= 205$$ DAI
* Owner = John 

### Adding liquidity

#### 1\) Calculate Factors

####  1.1\) Calculate new price \($$P_i$$\)

The price will be calculated by the BS contract and will return a unit price, $$P_i$$. Consider that the unit price calculated was 2 DAI per option for this example.

$$P_i=2$$

#### 1.2\) Calculate pool's value factor

No inventory imbalance means $$Fv_i=1$$ :

$$\displaystyle Fv_i= \frac{(TB(A)_{i-1}\cdot P_i+TB(B)_{i-1})}{(DB(A)_{i-1} \cdot P_i+DB(B)_{i-1})}= 1$$ 

$$TB(A)_{i-1}=DB(A)_{i-1} $$ 

$$TB(B)_{i-1}=DB(B)_{i-1}$$ 

#### 2\) Updates

#### 2.1\) Update User Balance

$$UB(A)_u=A_i$$ 

$$100 = A_i$$ 

$$UB(B)_u=B_i$$ 

$$205=B_i$$ 

#### 2.2\) Update Pool Factor in the moment of the user's deposit

$$UB(F)_u=1$$ 

#### 2.3\) Update total balances of the pool

$$TB(A)_i=TB(A)_{i-1} +A_{du}$$ 

$$TB(A)_i=0+100 = 100$$ 

$$TB(B)_i=TB(B)_{i-1} +B_{du}$$ 

$$TB(B)_i=0 +205=205$$ 

#### 2.4\) Update deamortized balance of the pool for each factor

$$\displaystyle DB(A)_i=DB(A)_{i-1} +\frac{A_{du}}{Fv_i}$$ 

$$DB(A)_i=0+\frac{100}{1}=100$$ 

$$\displaystyle DB(B)_i=DB(B)_{i-1} +\frac{B_{du}}{Fv_i}$$ 

$$DB(B)_i=0+\frac{205}{1}=205$$ 

{% hint style="success" %}
John Add liquidity ✅
{% endhint %}

#### Example Information

The event `trade` will happen in the instant  $$i= 1$$ given the following information:

* $$A_du=-2$$ options \(negative to show that the options will leave the contract\)
* Trade direction = user wants to receive exact amount of token A \(trade function will follow the `exactAOutput` setup\)
* Max price slippage 20%
* Owner = Gui 

### Trade

#### 1\) Calculate Factors

####  1.1\) Calculate new price \($$P_{i+1}$$\)

The price will be calculated by the BS contract and will return a unit price, $$P_i$$. Consider that the unit price calculated was 4 DAI per option for this example.

$$P_i=4$$

#### 1.2\) Calculate total price based on transaction amount

#### a\) Calculate pool amounts for each token:

$$\displaystyle poolAmountA = min(TB(A);\frac{TB(B)}{P_i})$$ 

$$poolAmountA=min(100;\frac{205}{4})$$ 

$$poolAmount A=51.25$$ 

$$poolAmountB=min(TB(B);TB(A)\cdot P_i)$$ 

$$poolAmountB=min(205;100\cdot 4)$$ 

$$poolAmountB=205$$ 

#### b\) Calculate product constant

$$k=poolAmountA\cdot poolAmountB$$ 

$$k=51.25\cdot 205= 10,506.25$$ 

#### c\) Calculate total transaction price in terms of token B

#### $$\displaystyle B_i=\frac{k}{(poolAmountA-tradeAmountA)}-poolAmountB$$ 

$$B_i= \frac{10,506.25}{(51.25-2)}-205 $$ 

$$B_i=213.32-205= 8.32$$ 

#### 2\) Updates

#### 2.1\) Update Total Balances

$$TB(A)_i=TB(A)_{i-1} +A_{du}$$ 

$$TB(A)_i=100+ (-2)=98$$

$$TB(B)_i=TB(B)_{i-1} +B_{du}$$ 

$$TB(B)_i=205+8.32= 213.32$$ 

{% hint style="success" %}
Gui bought options   
Trade  ✅
{% endhint %}

### Adding Liquidity  \(2nd time\)

Consider that a second user, Bob, wants to add liquidity to the pool at this moment. 

#### Example Information

The event `adding liquidity` will happen in the instant  $$i= 0$$ with the following information:

* $$A_{du}=50$$ options
* $$B_{du}= 30$$ DAI
* Owner = John 

#### 1\) Calculate Factors

####  1.1\) Calculate new price \($$P_i$$\)

The price will be calculated by the BS contract and will return a unit price, $$P_i$$. Consider that the unit price calculated was 3 DAI per option for this example.

$$P_i=3$$

#### 1.2\) Calculate pool's value factor

There is an inventory imbalance, and $$Fv_i$$ will be different from 1. 

$$\displaystyle Fv_i= \frac{(TB(A)_{i-1}\cdot P_i+TB(B)_{i-1})}{(DB(A)_{i-1} \cdot P_i+DB(B)_{i-1})}$$ 

$$Fv_i=\frac{(98\cdot3+213.32)}{(100\cdot 3+205)}= 1.00459406$$ 

#### 2\) Updates

#### 2.1\) Update User Balance

$$UB(A)_u=A_i$$ 

$$50 = A_i$$ 

$$UB(B)_u=B_i$$ 

$$30=B_i$$ 

#### 2.2\) Update Pool Factor in the moment of the user's deposit

$$UB(F)_u=1.00459406$$ 

#### 2.3\) Update total balances of the pool

$$TB(A)_i=TB(A)_{i-1} +A_{du}$$ 

$$TB(A)_i=98+50 = 148$$ 

$$TB(B)_i=TB(B)_{i-1} +B_{du}$$ 

$$TB(B)_i=213.32+30=243.32$$ 

#### 2.4\) Update deamortized balance of the pool for each factor

$$\displaystyle DB(A)_i=DB(A)_{i-1} +\frac{A_{du}}{Fv_i}$$ 

$$DB(A)_i=100+\frac{50}{1.00459406}=149.7713$$ 

$$\displaystyle DB(B)_i=DB(B)_{i-1} +\frac{B_{du}}{Fv_i}$$ 

$$DB(B)_i=205+\frac{30}{1.00459406}=234.8628$$ 

{% hint style="success" %}
Bob Add liquidity ✅
{% endhint %}

### Remove liquidity

Assuming that John decides to remove liquidity and that the price after changed again. 

#### 1\) Calculate new price 

The price will be calculated by the BS contract and will return a unit price, $$P_i$$. Consider that the unit price calculated was 2 DAI per option, equal to the previous period. 

$$P_i=2$$ 

#### 2\) Calculate the Pool's opening factor

Since there was a trade, the factors TB\(A\) and TB\(B\) are no longer equal to DB\(a\) and DB\(B\). Therefore, the pool's opening factor will be different from 1.

$$\displaystyle Fv_i= \frac{(TB(A)_{i-1}\cdot P_i+TB(B)_{i-1})}{(DB(A)_{i-1} \cdot P_i+DB(B)_{i-1})}$$

$$Fv_i=\frac{(148\cdot 2+243.32)}{(149.7713\cdot2+234.8628)}=1.00919639$$  

#### 2.2\) Calculate multipliers

$$\displaystyle mAA_i= \frac{min(Fv_i\cdot DB(A)_{i-1};TB(A)_{i-1})}{DB(A)_{i-1}}$$ 

$$mAA_i=min(1.00919639*149.7713;148) /149.7713=0.98817$$ 

$$\displaystyle mBB_i= \frac{min(Fv_i\cdot DB(B)_{i-1};TB(B)_{i-1})}{DB(B)_{i-1}}$$

$$mBB_i=min(1.00919639*234.8628;243.32) /234.8628 =1.00919639$$ 

$$\displaystyle mAB_i= \frac{TB(B)_{i-1}-mBB_i\cdot DB(B)_{i-1}}{DB(A)_{i-1}}$$ 

$$mAB_i=(243.32-(1.03600911*234.8628))/149.7713=0$$ 

$$\displaystyle mBA_i= \frac{TB(A)_{i-1}-mAA_i\cdot DB(A)_{i-1}}{DB(B)_{i-1}}$$ 

$$mBA_i=(148-(0.98817*149.7713))/234.8628=0$$ 

#### 2.3\) Calculate the withdrawal amount of each token

The following will happen considering a 100% withdrawal of the liquidity provided initially on both tokens:

$$\displaystyle A_i=-[mAA_i\cdot r_a\cdot \frac{UB(A)u}{Fv_{du}}+mBA_i\cdot r_b\cdot \frac {UB(B)u}{Fv_{du}}]$$ 

$$A_i=-[0.98817\cdot 1\cdot(\frac{100}{1})+0\cdot 1\cdot (\frac{205}{1})]=-98.17$$ 

$$\displaystyle B_i=-[mBB_i\cdot r_b\cdot \frac{UB(B)u}{Fv_{du}} + mAB_i \cdot r_a\cdot \frac {UB(A)_u}{Fv_{du}}] $$

$$B_i=-[1.03600911\cdot 1\cdot (\frac{205}{1})+0\cdot1\cdot (\frac{100}{1})]=-212.38$$ 



{% hint style="info" %}
Trade followed by price moves can cause a change in the pool's value and proportion. The impermanent loss or gain is likely in this scenario, and it is fairly distributed among liquidity providers.
{% endhint %}



