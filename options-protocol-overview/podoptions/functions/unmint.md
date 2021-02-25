---
description: >-
  Unminting an option is similar to the withdraw function, check below the major
  differences.
---

# Unmint

## Unmint

The unmint function is a function to assist options sellers that decide to leave the position before expiration.

To do so, the option seller has to buy the equivalent amount of options in the secondary market \(in the options AMM\) and use them together with the address that it holds \(`owner`\) the position to unmint.

‌Note that this leaves the seller on the option price exposure because it can happen that when they decide to leave the position, the options in the market are above the premium they received. "Buying back" the options at a higher price to leave a position can incur a loss.

Unmint function is very similar to the withdraw function. The differences are:

* Withdraw function can only happen after expiration.
* The withdraw function only allows users to withdraw 100% off the funds.
* In European options, unmint will never result in removing two assets \(strike asset and underlying asset\). Instead, unmint will only return the funds that were previously provided as collateral since options weren't exercised yet. 
* The withdraw function can return a combination of assets \(underlying and strike asset\) since a few buyers may exercise the options during the expiration window.

The event of unminting options requires the following information:  
1. Amount of options to unmint  
2. Owner

After the information was supplied to the contract, the `unmint` function will perform the following activities:

### 1\) Consult Balances 

### 1.1\) Consult current $$StrikeReserves_n$$

This step will fit its purpose better in a put option, where the collateral is in strike asset.

This balance should reflect the new current strikeReserves, considering the interest that could accrue in the meanwhile. We are calling `balanceOf()`from `ERC20` strike asset to check option contract strike asset balance.

### 1.2\) Consult current $$UnderlyingReserves_n$$

$$UnderlyingReserves_n$$ is a variable where we store the total amount of underlying assets currently locked in the protocol. This function has a precise fit with call options since the underlying asset is the asset locked as collateral in call options. We call`balanceOf()`from an `ERC20` underlying asset to check the option contract's strike asset balance.

### 2\) Calculate factors

### 2.1\) Calculate $$OwnerShares_w$$

This step calculates the proportional shares the user will unmint from its current position, considering his contract shares. 

$$\displaystyle OwnerShares_w=\frac{AmountOfOptionsToWithdraw\cdot OwnerShares_{i-1}}{UserMintedOptions}$$

### 2.2\) Calculate $$StikeToSend$$

This step is most fit with put options since the collateral in put options is in strike asset.

This step calculates the number of strike assets a user would need to receive for this unminting process. 

$$\displaystyle StrikeToSend=\frac{OwnerShares_w\cdot StrikeReserves_n}{TotalShares_{i-1}}$$ 

### 2.3\) Calculate $$UnderlyingToSend$$

This step is most fit with call options since the collateral in calls is in the underlying asset.

### 3\) Updates

### 3.1\) Update $$TotalShares_i$$

$$TotalShares_i$$ is the sum of all the user's shares.

$$TotalShares_i = TotalShares_{i-1} -OwnerShares_w $$

Notice the negative sign here is different from the pure mint function since it removed funds and, therefore, should reduce the number of shares.

### 3.2\) Update  $$OwnerShares_i$$

This factor updates the current total amount of user shares.

$$OwnerShares_i=OwnerShares_{i-1}-OwnerShares_w $$ 

### 3.3\) Update$$strikeReserves_n$$

They are used in the case of a put option.

Using the current $$StrikeToSend$$ \(accrued with interest from the last period\), we'll deduct the amount of strike to transfer used while minting this option.

#### $$StrikeReserves_i=StrikeReserves_{i-1} - StrikeToSend$$ 

{% hint style="info" %}
This session described mathematically how the contract logic works. It is not needed to update the total ERC20 balances at the code level since tokens transfer to or from the contract is automatically updated with the ERC20 implementation.  
{% endhint %}

### 3.4\) Update $$UnderlyingReserves_i$$

They are used in the case of a call option.

Using the current $$UnderlyingReserves_n$$ \(accrued with interest from the last period\), we'll deduct the amount of $$UnderlyingToSend$$ used while minting this option.

#### $$UnderlyingReserves_i=UnderlyingReserves_n-UnderlyingToSend$$ 

{% hint style="info" %}
This session described mathematically how the contract logic works. It is not needed to update the total ERC20 balances at the code level since tokens transfer to or from the contract is automatically updated with the ERC20 implementation.  
{% endhint %}

### 4\) Burn options

After the unmint function is processed, the options tokens will be burned.

{% hint style="success" %}
Unminting options ✅
{% endhint %}



