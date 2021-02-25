---
description: Understanding the minting function step-by-step.
---

# Mint

## Mint

The event of minting an option requires the following initial information:  
1. Option Amount  
2. Owner

After the information was supplied to the contract, the `mint` function will perform the following activities:

### 1\) Required Collateral Amount

Calculate the number of assets the user has to send to the contract to lock as collateral. 

If this is a **put** option:  


$$AmountToTransfer=OptionsAmount\cdot StrikePrice$$ 

If this is a **call** option:  


$$AmountToTransfer=OptionsAmount\cdot UnderlyingAsset$$ 

### 2\) Consult Balances

### 2.1\) Consult Current $$StrikeReserves_n$$

$$StrikeReserves_n$$ is a variable where we store the current amount of the strike asset. If the strike asset is an interest-bearing token, it is expected that $$StrikeReserves_n$$ will have accrued some interest since the last period. We are calling `balanceOf()` from `ERC20` strike asset to check option contract strike asset balance.

$$StrikeReserves_n=StrikeReserves_{i-1}+aTokensYield $$

### 2.2\) Consult Current $$UnderlyingReserves_n$$

$$UnderlyingReserves_n$$is a variable where we store the total amount of underlying assets currently locked in the protocol. This function has a precise fit with call options since in call options, the underlying asset is the asset locked as collateral. We are calling `balanceOf()` from `ERC20` an underlying asset to check option contract strike asset balance.

### 3\) Calculate $$OwnerShares_i$$

$$OwnerShares_i$$ is a variable that stores the shares of the user based on the current option contract situation. 

If $$i=0,  OwnerShares_i=AmountToTransfer$$ 

If $$i≠0,$$we identify the following cases:

If this is a **put** option:

$$\displaystyle OwnerShares_i=\frac{AmountToTransfer\cdot TotalShares_{i-1}}{StrikeReserves_n+(UnderlyingReserves_i \cdot StrikePrice)}$$ 

If this is a **call** option:

$$\displaystyle OwnerShares_i=\frac{{AmountToTransfer\cdot TotalShares_{i-1}}}{UnderlyingReserves_i+\frac{StrikeReserves_i}{StrikePrice}}$$

### 4\) Updates

### 4.1\) Update  $$TotalShares_i$$

$$TotalShares$$ is the sum of all the owner's individual factor of $$OwnerShares$$.

### $$TotalShares_i = TotalShares_{i-1} +OwnerShares_i $$

### 4.2\) Update $$UserMintedOptions_i$$ 

Update the current number of outstanding options minted by the same user on the same option series.

$$UserMintedOptions=UserMintedOptions_{i-1}+OptionsAmount$$ 

### 4.3\) Update $$OwnerShares_i $$

This factor updates the current owner shares. Important: If the user had previously minted options, those would be accounted for as one factor.

$$OwnerShares_t =OwnerShares_{i -1}+OwnerShares_i $$ 

### 4.4\) Update $$StrikeReserves_i$$

Using$$StrikeReserves_n$$ \(accrued with interest from the last period\) we'll add the $$AmountToTransfer$$ to transfer used while minting this option.

#### $$StrikeReserves_i=StrikeReserves_n+AmountToTransfer$$ 

{% hint style="info" %}
This session described mathematically how the contract logic works. It is not needed to update the total ERC20 balances at the code level since tokens transfer to or from the contract is automatically updated with the ERC20 implementation.  
{% endhint %}



{% hint style="success" %}
Minting options ✅
{% endhint %}

