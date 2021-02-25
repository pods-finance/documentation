---
description: Options buyers can exercise their right during the window of expiration.
---

# Exercise

## Exercise

The event of exercising an option requires the following initial information:  
1. Amount To Exercise  
2. Owner

After the information was supplied, the `exercise` function will perform the following activities:

{% hint style="warning" %}
This function can only be called **during**`exerciseWindow`period, depending on the exercise type.

For **European:** after trade window and before expiration.  
For **American:** anytime before expiration.
{% endhint %}

### 1\) Consult Current $$StrikeReserves_n$$

This balance should reflect the new current strikeReserves, considering the interest that accrued in the meanwhile. We are calling `balanceOf()`from `ERC20` strike asset to check option contract strike asset balance.

### **2\) Calculate** $$StrikeToSend$$\*\*\*\*

This step calculates how many strike assets the user will receive in a put option after it requested the exercise.

$$StrikeToSend=ExerciseAmount\cdot StrikePrice$$ 

### 3\) Calculate $$UnderlyingToReceive$$

This step calculates the amount a user should receive in underlying assets after the exercise was requested in the case of a call option.

$$UnderlyingToReceive=ExerciseAmount$$

### 4\) Updates

### 4.1\) Update  $$StrikeReserves_i$$

Using the current $$StrikeReserves$$ \(accrued with interest from the last period\), we'll deduct the amount  $$StrikeToSend$$ used while minting this option.

#### $$StrikeReserves_i=StrikeReserves_n-StrikeToSend$$ 

### 4.2\) Update $$underlyingReserves_i$$

Using the current $$UnderlyingReserves$$ \(accrued with interest from the last period\), we'll add the amount of underlying to transfer used while minting this option. Also, now with the expiration, one can either receive tokens or send tokens. In the case of a put option, this balance should increase if the options were exercised. That means an option buyer chose to exercise options.

$$UnderlyingReserves_i=UnderlyingReserves_n+UnderlyingToReceive$$ 

### 5\) Burn options

Burn exercised options.

$$
Burn = ExerciseAmount
$$

Note that exercise functions do not impact the $$TotalShares $$or any $$OwnerShares $$ .



{% hint style="success" %}
Exercise options ✅
{% endhint %}

