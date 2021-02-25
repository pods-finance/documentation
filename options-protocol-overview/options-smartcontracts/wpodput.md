# WPodPut

WPodPut is the PodPut version made for dealing when the `underlying asset`  is ETH. Under the hood, it uses WETH in the option contract. 

The functions `mint`, `unmint`, and `withdraw` can be used the same way as described on [PodPut](/@pods-finance-1/s/teste/~/drafts/-MU616VUJSGVBIAmhrla/options-protocol-overview/options-smartcontracts/sm-podput-1), but instead of receiving an ERC20 as payment, `msg.sender` could receive `ETH` instead. The only function with different signature will be `exerciseEth`

## Methods

### exerciseETH

Allow put option token holders to use their option tokens to sell some `amountOfOptions` units of the `underlying asset` for the`amountOfOptions * strike price` units of the strike token. Unlike the exercise used on PodPut, the caller should send ETH in the ratio of 1:1 with`amountOfOptions.` The contract will check if the `msg.value` is equal to the `amountOfOptions.` Otherwise, the transaction will revert.

During the process:

* The `amountOfOptions * strikePrice` units of `strike asset` are transferred to the caller
* The `amountOfOptions` option tokens are burned.

{% hint style="warning" %}
This function can only be called **during**`expirationWindow` period, depending on the exercise type.

For **European:** after trade window and before expiration.  
For **American:** anytime before expiration.
{% endhint %}

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option
PodPut podPut = PodPut("/*address*/");

uint256 amountOfOptions = 10000000;
uint256 ethAmount = amountOfOptions;

// @param amountOfOptions The amount option tokens to be exercised
podPut.exercise.value(ethAmount)(amountOfOptions);
```
{% endtab %}
{% endtabs %}

