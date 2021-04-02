---
description: >-
  Puts and calls share the same logic, have the same structure, and access the
  same functions.
---

# Options Instrument

## Create

Since the logic and structure of PodOptions \(calls and puts\) are similar, one can use a generic factory and constructor for deploying these options.

### Parameters

To create a brand new option is required to fill the following parameters:

| Input Name | Description |
| :--- | :--- |
| `name` | The option token name \(e.g., Pod BTC:USDC 300\) |
| `symbol` | The option token symbol \(e.g., "PodPutBTC"\) |
| `exercise type` | 0 for American / 1 for European |
| `underlying asset` | Underlying asset address |
| `strike asset` | Strike asset address. It is enabled for interest-bearing tokens, Aave's aToken. |
| `strike price` | The strike price \(units of strikeAsset that buy 1 unit of underlying\) \(e.g., 12000000000 means you need 12000000000 units of strike asset to exercise 1 unit of WBTC.\) |
| `expiration` | Expiration option date in UNIX timestamp \(e.g., 1609401600\) |
| `(exercise) window size` | Duration of the exercise windows in seconds. The period that the buyer will have to exercise. |

Note that there are **no outside requirements** to set up a new option contract.

Besides general required properties and general functions, PodOptions contract implies that:

* Options tokens are fungible ERC20 within each option series. 
* **Burn** function only applies in the exercise and unmint functions.
* Options tokens are minted in a 1:1 parity with the underlying asset.

Exercising an option is never blocked at the contract level. It means that if a user wants to exercise an _out-of-the-money_ option, the user will succeed, and that will translate into a loss at the moment \(will buy an asset at a higher price than spot price or will sell an asset for a price that is lower then spot price\).

> For further details on how to create an option token, check the factory and constructor guides.

## Examples

### PodCall Example

Let's explore the example of setting up a call option.

{% hint style="info" %}
**Quick recap:** to the buyer, a PodCall represents the **right to buy** the underlying asset upon expiration.
{% endhint %}

* **name:** Pods Call Option ETH:aUSDC 480 December 30th 8 AM GMT
* **symbol:** PodCallETH:aUSDC:480
* **type:** Call
* **exercise type**: European
* **underlying asset:** ETH
* **strike asset:** aUSDC
* **strike price:** 480 aUSDC
* **expiration:** December 30th
* **window size:** 24 hours.

The amount of collateral that should be held in a call option contract is:

$$CallCollateral = Underlying Asset*MintedOptions$$

### PodPut Example

Let's explore the example of setting up a call option.

{% hint style="info" %}
**Quick recap:** to the buyer, a PodPut represents the **right to sell** the underlying asset upon expiration.
{% endhint %}

* **name:** Pods Put Option ETH:aUSDC 500 December 30th 8 AM GMT
* **symbol:** PodPutETH:aUSDC:500
* **type:** Put
* **exercise type**: European
* **underlying asset:** ETH
* **strike asset:** aUSDC
* **strike price:** 500 aUSDC
* **expiration:** December 30th
* **window size:** 24 hours.

The amount of collateral that should be held in a put option contract is:

$$PutCollateral = StrikePrice*MintedOptions$$

### Comparing Collateral

Note that the parameters are the same for calls and puts, and what changes is the collateral asset. Since we are doing physical settlement, it is crucial to make sure the options hold 100% of the maximum amount that the contract represents. Both calls and puts have to be fully collateralized.

#### Calls

Since calls represent the right to buy the underlying asset in the future, the contract must hold the asset that the buyer may want to buy. That's why the **collateral for calls** is the actual **underlying asset** itself.

#### Puts

Since puts represent the right to sell the underlying asset in the future, the contract must hold the total amount for maybe buying that asset in the future. That's why the collateral for puts translates into `strike asset * strike price * minted options.`

{% hint style="success" %}
Now that you know how to create new options let's check their functions.
{% endhint %}

