# PodCall

PodCall is a contract that should inherit PodOption and override the four main functions: `mint`, `unmint`, `exercise`, and `withdraw.`

## Methods

### mint

This function is meant to be called by underlying token holders wanting to write option tokens.

Locks some amount of the underlying asset and writes option tokens. The issued amount ratio is 1:1 \(e.g., 1 option token for 1 underlying token\). It presumes the caller has already called`IERC20.approve()` on the underlying token contract to move caller funds. 

{% hint style="warning" %}
This function can only be called **during** `tradeWindow` period depending on the exercise type.

For **European:** before `exerciseWindow`.  
For **American:** anytime before expiration.
{% endhint %}

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option
PodCall podCall = PodCall("/*address*/");

// Number of options to mint. The decimals of the options will be the same
// as the decimals of the underlying asset.
// (E.g: If option decimals is equal to 6, 1000000 = 1 option, 
// givig the seller the right to buy 1 unit of underlying)
uint256 amountOfOptions = 1000000;

/*
* @param amountOfOptions The amount option tokens to be issued; this will lock
* same amount in a 1:1 ratio units of underlying asset into this
* contract.

* @param owner The address that will store at shares. owner will
* be able to withdraw later on behalf of the sender. It is also important 
* to notice that the options tokens will not be send to owner, but to the msg.sender
*/
podCall.mint(amountOfOptions, owner);
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Soon
```
{% endtab %}
{% endtabs %}

### unmint

Unlocks some amount of the underlying token by burning option tokens. This mechanism ensures that users can only redeem tokens **they've previously lock** into this contract.

{% hint style="warning" %}
This function can only be called **during** `tradeWindow` period depending on the exercise type.

For **European:** before `exerciseWindow`.  
For **American:** anytime before expiration.
{% endhint %}

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option
PodCall podCall = PodCall("/*address*/");

uint256 amountOfOptions = 10000000;

/*
 * @param amountOfOptions The amount option tokens to be unminted; this will burn
 * same amount of options, releasing in a 1:1 ratio units of underlying asset.
*/
podCall.unmint(amountOfOptions);
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Soon
```
{% endtab %}
{% endtabs %}

### exercise

Allow call token holders to use them to buy some `amountOfOptions` units of the `underlying token` for the `amountOfOptions * strike price` units of the strike token. It presumes the caller has already called `IERC20.approve()` on the strike token contract to move caller funds.

During the process:

* The `amountOfOptions` units of `underlying tokens` are transferred to the

     caller

* The `amountOfOptions` option tokens are burned.
* The `amountOfOptions * strikePrice` units of strike tokens are transferred into

  this contract as a payment for the underlying tokens.

{% hint style="warning" %}
This function can only be called **during**`expirationWindow` period depending on the exercise type.

For **European:** after trade window and before expiration.  
For **American:** anytime before expiration.
{% endhint %}

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option
PodCall podCall = PodCall("/*address*/");

uint256 amountOfOptions = 10000000;

// @param amountOfOptions The amount option tokens to be exercised
podCall.exercise(amountOfOptions);
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Soon
```
{% endtab %}
{% endtabs %}

### withdraw

Withdraws the locked amount of Strike tokens after expiration by burning Options.

The Strike Asset is **NOT** withdrawn on a first-come-first-serve basis. Meaning that, if there is not enough `underlying asset` left because the series has been exercised, The mix of `underlying tokens` and `strike asset`are distributed proportionally to the sellers.

{% hint style="warning" %}
This function can only be called **during** `withdrawWindow` which is after expiration.
{% endhint %}

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option
PodCall podCall = PodCall("/*address*/");

// Withdraws the original position. Could be only underlying asset,
// a mix of underlying asset and strike, or only strike asset.
podCall.withdraw();
```
{% endtab %}
{% endtabs %}

