# PodPut

PodPut is a contract that should inherit PodOption and override the 4 main functions: `mint`, `unmint`, `exercise` and `withdraw.`

## Methods

### mint

This function is meant to be called by strike token holders wanting to write option tokens.

Locks some amount of the strike asset and writes option tokens. The issued amount ratio is 1 option token for `strikePrice`units of strike asset. It presumes the caller has already called `IERC20.approve()` on the strike token contract to move caller funds.

{% hint style="warning" %}
This function can only be called **during** `tradeWindow` period depending on the exercise type.

For **European:** before `exerciseWindow`.  
For **American:** anytime before expiration.
{% endhint %}

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option
PodPut podPut = PodPut("/*address*/");

// Number of options to mint. The decimals of the options will be the same
// as the decimals of the underlying asset.
// (E.g: If option decimals is equal to 6, 1000000 = 1 option, 
// givig the seller the right to buy 1 unit of underlying)
uint256 amountOfOptions = 1000000;

/*
* @param amountOfOptions The amount of option tokens to be issued; 
* this will lock strikePrice * amountOfOptions of strike asset into this
* contract.
*
* @param owner The address that will store at shares. owner will
* be able to withdraw later on behalf of the sender. It is also important 
* to notice that only the shares will be store to the owner.
* The options tokens wil not be send to owner, but to the msg.sender
*/
podPut.mint(amountOfOptions, owner);
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Soon
```
{% endtab %}
{% endtabs %}

### unmint

Unlocks some amount of the strike token by burning option tokens. This mechanism ensures that users can only redeem tokens **they've previously lock** into this contract

American options where exercise can happen anytme before the expiration, the caller may receive a mix of the `underlying asset` and `strike asset`.

{% hint style="warning" %}
This function can only be called **during** `tradeWindow` period depending on the exercise type.

For **European:** before `exerciseWindow`.  
For **American:** anytime before expiration.
{% endhint %}

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option
PodPut podPut = PodPut("/*address*/");

uint256 amountOfOptions = 10000000;

/*
 * @param amountOfOptions The amount option tokens to be unminted; this will burn
 * same amount of options, releasing strikePrice units of strike asset.
*/
podPut.unmint(amountOfOptions);
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Soon
```
{% endtab %}
{% endtabs %}

### exercise

Allow put token holders to use them to sell some `amountOfOptions` units of the `underlying asset` for the `amountOfOption * strike price` units of the strike token. It presumes the caller has already called `IERC20.approve()` on the `underlying token` contract to move caller funds.

During the process:

* The `amountOfOptions * strikePrice` units of `strike asset` are transferred to the caller
* The `amountOfOptions` option tokens are burned.
* The`amountOfOptions` units of `underlying asset`are transferred into

  this contract as a payment for the `strike asset`.

{% hint style="warning" %}
This function can only be called **during**`expirationWindow` period depending on the exercise type.

For **European:** after trade window and before expiration.  
For **American:** anytime before expiration.
{% endhint %}

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option
PodPut podPut = PodPut("/*address*/");

uint256 amountOfOptions = 10000000;

// @param amountOfOptions The amount option tokens to be exercised
podPut.exercise(amountOfOptions);
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Soon
```
{% endtab %}
{% endtabs %}

### withdraw

Withdraws the locked amount of `strike asset` after expiration by burning options.

The Strike Asset is **NOT** withdrawn on a first-come-first-serve basis. Meaning that, if there is not enough `strike asset` left because the series has been exercised, The mix of `underlying tokens` and `strike asset`are distributed proportionally to the sellers.

{% hint style="warning" %}
This function can only be called **during** `withdrawWindow` which is after expiration.
{% endhint %}

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option
PodPut podPut = PodPut("/*address*/");
/*
* Withdraws the original position. Could be only underlying asset,
* a mix of underlying asset and strike, or only strike asset
* depending on how the pool was exercised.
*/

podPut.withdraw();
```
{% endtab %}
{% endtabs %}

