---
description: This page will go over the components of the AMM and how they interact.
---

# Components

## AMM Properties

The Options AMM has four properties:

1. The AMM will programmatically update the IV to reduce arbitrage opportunities. 
2. The AMM allows for single-sided liquidity provision using exposure and not position point of view.
3. If there is no inventory imbalance, canceling trades \(not reflecting any arbitrage opportunity\) or price changes due to price calculations should not take nor add any value from or to the liquidity providers.
4. Any gain or loss of the AMM pool should impact the LPs fairly across time.

The AMM is structured in 5 components that keep these properties over the lifetime of the pool. Find below a description of those components and their concepts.

### Pool Value Factor

The Pool Value Factor short representation is $$F_{v_{i}}$$. It aims to express the current pool circumstances in one number as if it was a snapshot. The Pool Value Factor is essential to keep the distribution of impermanent loss or gain correctly among users and the moment they entered the pool. 

Consider the case: when a user added liquidity to the pool, the $$F_{v_{i}}$$at that moment was 0.9. This number will be stored in the $$UB_{F_{u}}$$ variable \(explained below\) within the user's struct. By the time the user wishes to remove the funds, the $$UB_{F_{u}}$$ will be compared to the current $$F_{v_{i}}$$of the pool, let's say now the factor is at 1. That will represent that the user had an impermanent gain of 11%  \($$\frac {1}{0.9}$$\). In the Multipliers component \(explained below\), this will be used to calculate the delivered position for the user to express this given the pool conditions.

In short, we need this number to compare how the pool changed over time. By the moment of withdrawing, we'll compare the pool's factor to the user's factor \(got on a snapshot at the moment of entry into the pool\) and estimate if that resulted in a gain or loss. The user can experience an impermanent loss or gain depending on the moment they entered and what happened to the pool while they were there. It is important to highlight that even if a pool has an impermanent loss, a user that just joined will not face immediate impermanent loss. Instead, each user will have its own path of impermanent loss or gain calculation in a pool.

The pool's value factor \($$F_{v_{i}}$$\) is always equals to 1 in the opening of the pool, representing initial balance in the pool.   
If $$i=0$$ , $$Fv_i=1$$ 

Actions such as adding liquidity, re-add liquidity, and remove liquidity do not impact the $$F_{v_{i}}$$ since they impact equally the factors of $$TB$$and $$DB$$. Instead, what impacts this factor is the trades that may happen in the pool. 

If $$i≠0$$ , $$\displaystyle Fv_i= \frac{TB_{A_{i-1}}\cdot P_i+TB_{B_{i-1}}}{DB_{A_{i-1}} \cdot P_i+DB_{B_{i-1}}}$$  

### User Component

A user$$u$$ that provides liquidity in either the options tokens or stablecoin has three different properties saved as a struct during the life of the pool:

1. $$UB_{A_{u}}$$ -&gt; Is short for User Balance of Token A. This factor holds the current amount of tokens A the user $$u$$ has provided as liquidity. It is the result of the initial liquidity provision and/or a re-add liquidity event. 

2. $$UB_{B_{u}}$$-&gt; Is short for User Balance of Token B. This factor holds the current amount of tokens B the user $$u$$ has provided as liquidity. It is the result of the initial liquidity provision and/or a re-add liquidity event. 

3. $$UB_{F_{u}}$$-&gt; User Balance Pool Factor is the variable that takes a snapshot of the Pool Factor \($$F_{v_{i}}$$\) by the time the user last added liquidity. This factor will be responsible for making sure the user keeps its initial exposure over time. 

The user component \(represented in the struct\) is updated in two circumstances:

* If this is the first time this user adds liquidity or,
* If this is not the first time this user adds liquidity. 

In the case of the first liquidity provision,  $$UB_{A_{u}}$$and $$UB_{B_{u}}$$ will be equal to the amount that is being provided as liquidity, $$A_{du}$$ and $$B_{du}$$ respectively. 

$$UB_{A_{u}}=A_{du}$$ 

$$UB_{B_{u}}=B_{du}$$

$$UB_{F_{u}}=F_{v_i}$$

In the latter case, where the user is re-adding liquidity, the $$UB_{A_{u}}$$and $$UB_{B_{u}}$$ will be updated differently. It considers the funds previously added, multiplied by a factor that takes into account both the current Pool Factor \( $$F_{v_{i}}$$ \) and the User Balance Pool Factor \($$UB_{F_{u}}$$\). This could be understood as if the pool is updating the virtual user balance to the current pool state. 

$$\displaystyle UB_{A_{u}}=UB_{A_{u_{i-1}}}\cdot \frac {F_{v_{i}}}{UB_{F_{u}}}+A_{du}$$ 

$$\displaystyle UB_{B_{u}}=UB_{B_{u_{i-1}}}\cdot \frac {F_{v_{i}}}{UB_{F_{u}}}+B_{du}$$ 

$$UB_{F_{u}}=F_{v_i}$$

### Pool's Deamortized Balances

The Deamortized Balance, $$DB_{A_i}$$and $$DB_{B_i}$$, are representations of what the pool owe's to the LPs. That could be seen as storing the original deposits provided by the LPs. 

They are calculated upon the events of adding liquidity, re-adding liquidity, and removing liquidity. 

Find below the formulas for the Pool's deamortized balances:

$$\displaystyle DB_{A_{i}}=DB_{A_{i-1}} +\frac{A_{du}}{F_{v_{i}}} $$

$$\displaystyle DB_{B_{i}}=DB_{B_{i-1}} +\frac{B_{du}}{F_{v_{i}}}$$

By the time the pool is created \($$i=0$$\) , the $$DB_{A_i}$$and $$DB_{B_i}$$ will be equal $$A_{du}$$ and $$B_{du}$$ since:

 $$F_{v_{i}} = 1$$  
$$\displaystyle DB_{A_{i-1}}=0$$  
$$\displaystyle DB_{B_{i-1}}=0$$

By the time any other even happen \(except trades\), by any user, the $$DB_{A_i}$$and $$DB_{B_i}$$ will be updated to reflect the amount of original deposits the pool received since the beginning. 

The additional amounts of tokens \(either, $$A_{du}$$ or $$B_{du}$$\) are added to the total $$DB_{A_i}$$and $$DB_{B_i}$$ by being divided by the current $$F_{v_{i}}$$. By doing so, the pool can take the new deposits to the same level that the previous deposits were, considering the pool's impermanent loss or gain.

### Total Balances

The Total Balances is basically the pool balance,  $$TB_{A_{i}}$$ and $$TB_{B_{i}}$$. They are constantly updated and represent the exact and current amount of each token that the pool holds. This factor will be contrasted against the Deamortized Balance, which will guide the calculation for the next component \(Multipliers\).

This factor is update whenever there is an event in the pool \(add liquidity, re-add liquidity, trade, and remove liquidity\).

{% hint style="info" %}
This variable is just a virtual conception of what it represents. It does not exist like that in the code. Instead, the code checks ERC20 option token addresses the current balance of each token to serve as input for formulations. 
{% endhint %}

### Multipliers

The multipliers are factors that the AMM calculates when a user calls the remove liquidity function. They will calculate the "delivered" position in the withdraw, considering the user's initial exposure. They can be understood as:

$$mAA_i$$ → How much A the users can remove for each A initially deposited

$$mBB_i$$ → How much B the users can remove for each B initially deposited

$$mAB_i$$ → How much B the users can remove for each A initially deposited

$$mBA_i$$→ How much A the users can remove for each B initially deposited

The multipliers function ensures that the original deposit is considered when calculating the withdrawal amount and that the withdrawal amount should never surpass the pool's total balances. They calculated the equivalent amount of tokens the user should receive to translate the initial exposure to withdrawal position. It is also expected that after all LPs removed the liquidity, the total balances of the pool will be zero. A multiplier could be equal to zero, and in that case, it describes that there is no position for that token on the withdrawal amount.

$$\displaystyle mAA_i= \frac{min(Fv_i\cdot DB_{A_{i-1}};TB_{A_{i-1}})}{DB_{A_{i-1}}}$$ 

$$\displaystyle mBB_i= \frac{min(Fv_i\cdot DB_{B_{i-1}};TB_{B_{i-1}})}{DB_{B_{i-1}}}$$ 

$$\displaystyle mAB_i= \frac{TB_{B_{i-1}}-mBB_i\cdot DB_{B_{i-1}}}{DB_{A_{i-1}}}$$ 

$$\displaystyle mBA_i= \frac{TB_{A_{i-1}}-mAA_i\cdot DB_{A_{i-1}}}{DB_{B_{i-1}}}$$ 

