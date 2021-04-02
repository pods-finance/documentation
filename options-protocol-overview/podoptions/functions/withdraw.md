---
description: >-
  Same process as unmint, but considering 100% of the funds and after exercise
  window.
---

# Withdraw

## Withdraw

The event of withdrawing funds from expired options doesn't require any parameters. It will unlock 100% of the seller's funds, which may be composed of a combination of strike assets and the underlying asset, depending on how many options were exercised.

There is **no** race condition on withdrawing funds. That means that it does not matter the order/moment you withdraw. The assets will be distributed evenly and proportionally between the owners.

The withdrawal function will perform the following actions after expiration.

### 1\) Consult Balances

### 1.1\) Consult Current  $$StrikeReserves_n$$

This step will fit its purpose better in a put option where the collateral is in strike asset.

This balance should reflect the new current $$StrikeReserves$$, considering the interest that accrued in the meanwhile.

### 1.2\) Consult Current $$UnderlyingReserves_n$$

$$UnderlyingReserves_n$$ is a variable where we store the total amount of underlying assets currently locked in the protocol. This function has a precise fit with call options since, in call options, the underlying asset is the asset locked as collateral.

### 2.1\) Calculate $$StrikeToSend$$

This step calculates the amount of strike asset an owner needs to receive for this withdrawal process.

$$\displaystyle StrikeToSend=\frac{OwnerShares_t\cdot StrikeReserves_n}{TotalShares_{i-1}}$$

### 2.2\) Calculate $$UnderlyingToSend$$

This step calculates the amount of underlying asset an owner needs to receive to complete the withdrawal.

$$\displaystyle UnderlyingToSend=\frac{OwnerShares_t\cdot UnderlyingReserves_n}{TotalShares_{i-1}}$$

### 3\) Updates

### 3.1\) Update $$TotalShares_i$$

$$TotalShares_i$$ is the sum of all the owner's shares.

$$TotalShares_i = TotalShares_{i-1} -OwnerShares_t$$

Notice that the negative sign here is different from the pure mint function since it removed funds and, therefore, removed the weighted impact.

### 3.2\) Update $$OwnerShares_t$$

This factor updates the current total amount of owner shares. This factor will store that the funds were withdrawn from the contract by setting it to zero.

$$OwnerShares_t=0$$

### 3.3\) Update  $$strikeReserves_i$$

Using $$StrikeReserves_n$$ \(accrued with interest from the last period\), we'll deduct the amount of $$StrikeToSend$$used while minting this option.

#### $$StrikeReserves_i=StrikeReserves_n-StrikeToSend$$

{% hint style="info" %}
This session described mathematically how the contract logic works. It is not needed to update the total ERC20 balances at the code level since tokens transfer to or from the contract is automatically updated with the ERC20 implementation.
{% endhint %}

### 3.4\) Update $$UnderlyingReserves_i$$

Using $$UnderlyingReserves_n$$ \(accrued with interest from the last period\), we'll deduct the amount of $$UnderlyingToSend$$ used while minting this option.

#### $$UnderlyingReserves_i=UnderlyingReserves_n-UnderlyingToSend$$

{% hint style="info" %}
This session described mathematically how the contract logic works. It is not needed to update the total ERC20 balances at the code level since tokens transfer to or from the contract is automatically updated with the ERC20 implementation.
{% endhint %}

{% hint style="success" %}
Withdraw options ✅
{% endhint %}

