# WPodCall

WPodCall is the PodCall version made for dealing when the `underlying asset`  is ETH. Under the hood, it uses WETH in the option contract. 

The `exercise, unmint ,`and `withdraw` can be used exactly the same way as described on [PodCall](sm-podput.md). The only function with different signature will be  `mintWithETH`

## Methods

### mintETH

Allow Call token holders to lock their `ETH` in a `1:1` ratio, meaning that 1 unit of `ETH` will mint 1 unit of option token  The contract will check if the `msg.value` is equal to the `amountOfOptions`. Otherwise, the transaction will revert.

It is possible to mint on behalf of someone. In that case, the `msg.sender` still receive the tokens, but the owner will own the position \(amount of `shares`\). With this `owner` will be able to withdraw his deserved amount of collateral by the right time. \(After expiration for American options, and after the end of exercise window for European\).

During the process:

* The caller will send `msg.value` amount of `ETH` to the `WPodCall` contract.
* The contract will check if `msg.value == amountOfOptions`asked. The transaction will revert if not.
* The contract will mint option tokens in a 1:1 ratio with the `ETH` transferred.
* The contract will store how many `shares` deserve and the `mintedOptions` amount.

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
uint256 ethAmount = amountOfOptions;
uint256 owner =  0x3.. // owner address

podPut.mintWithETH.value(ethAmount)(amountOfOptions, owner);

// WPodCall.sol
/**
     * @notice mint 1:1 units of options tokens based on the unit of ETH sent
     * @param amountOfOptions The amount of options to be minted
     * @param owner address of the owner that will hold the collateral position
    function mintWithETH(
        uint256 amountOfOptions,
        address owner
    ) external payable {}
```
{% endtab %}
{% endtabs %}

