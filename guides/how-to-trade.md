# How to Trade \(Buy/Sell\)

There are few ways to trade options from our pool. In this tutorial, we will focus on interacting directly with the [`OptionAMMPool`](../options-amm-overview/options-amm-sm/option-amm-pool.md). In order to do that, you will need to perform a few steps.

1. Find the `OptionAMMPool` address given a certain option using the `OptionAMMFactory`
2. Get the trade details given a certain options amount you want to trade using the `OptionAMMPool` with the function `getOptionTradeDetails`
3. Allow the `tokensB` \(stable tokens\) or the `tokensA` \(option tokens\) to be spent by the `OptionAMMPool`
4. Perform the trade

### 1. Find `OptionAMMPool`

You will need for this step:

1. OptionAMMFactory contract address
2. OptionAMMFactory interface or ABI
3. Option series address

You can also check this step directly on [getPool function](../options-amm-overview/options-amm-sm/option-amm-factory.md#getpool).

{% tabs %}
{% tab title="Solidity" %}
```javascript
pragma solidity >0.6.0;
// 1) Import IOptionAMMFactory interface
import "../interfaces/IOptionAMMFactory.sol";
// 2) Instantiate OptionAMMFactory.
IOptionAMMFactory optionAMMFactory = IOptionAMMFactory("/factoryAddress*/");

// 3) Get the option address you will want to buy.
address optionAddress = '0xe3...";

// 4) Call the getPool function and receive the pool address in return.
address optionAMMPoolAddress = optionAMMFactory.getPool(optionAddress);

```
{% endtab %}
{% endtabs %}

### 2. Get the Trade Details 

Now that you have the pool address from the previous step, you can call one of the following view functions:

| Function name | Description |
| :--- | :--- |
| `getOptionTradeDetailsExactAOutput` | You should pass as input the **exact** number of **options** you will want to **buy**. |
| `getOptionTradeDetailsExactAInput` | You should pass as input the **exact** number of **options** you will want to **sell**. |
| `getOptionTradeDetailsExactBOutput` | You should pass as input the **exact** number of **stable tokens** \(premium\) you will want to **receive** when **selling.** |
| `getOptionTradeDetailsExactBInput` | You should pass as input the **exact** number of **stable tokens** \(premium\) you will want to **pay** when **buying.** |

In return, you will receive the amount of `tokensB` \(stable assets\) ``or `tokensA` \(option tokens\) and `newIV`. `newIV` will be necessary later to perform the trade. 

{% tabs %}
{% tab title="Solidity" %}
```javascript
pragma solidity >0.6.0;
// 1) Import IOptionAMMFactory interface
import "../interfaces/IOptionAMMPool.sol";

// 2) Instantiate optionAMMPool using the address you
// got from the previous step
IOptionAMMPool optionAMMPool = IOptionAMMPool(optionAMMPoolAddress);


/*
 In this example we will perform a buyExactAOuput action.
 
 3) Call the getOptionTradeDetailsExactAOutput with
    the option amount you will want to buy as an input
    
    That function returns:
    uint256 amountBIn - The amount of tokenB in order to buy amountOfOptions
    uint256 newIV - The new pool Implied volatility, also known as sigma
*/

uint256 amountOfOptions = 100000; // The option decimals can be found calling option.decimals()
(uint256 amountBIn, uint256 newIV, , ,) = optionAMMPool.getOptionTradeDetailsExactAOutput(amountOfOptions);

```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
TokenA in the optionAMMPool will always be the option token. TokenB will always be the stable token. 
{% endhint %}

### 3. Allow the `tokensB` \(stable tokens\) or `tokensA` \(option tokens\) to be spent by the `OptionAMMPool`

If you are already familiar with Ethereum development, you can jump to step 4. We will only approve the `tokenA/tokenB` to be spent by the pool. If you are selling, you will need to approve`tokenA`, but if you are buying, you will need to approve `tokenB.`

{% tabs %}
{% tab title="Solidity" %}
```javascript
// 1) Get tokenB address from the pool
address tokenBAddress = optionAMMPool.tokenB();

// 2) Instantiate tokenB of the pool using ERC20 interface.
IERC20 tokenB = IERC20(tokenBAddress);

// 3) Approve optionAMMPool to spend amountToApprove 
// tokenB in your behalf

// You can use amountBIn from the previous step with
// an error.
// For example: (amountBIn + amountBIn / 10) // 10% tolerance
uint256 amountToApprove = 10000;

tokenB.approve(optionAMMPoolAddress, amountToApprove);

```
{% endtab %}
{% endtabs %}

### 4. Perform the trade

Now, finally, you are ready to perform the trade. You can check the full function specification [here](../options-amm-overview/options-amm-sm/option-amm-pool.md#tradeexactaoutput).

{% tabs %}
{% tab title="Solidity" %}
```javascript
/*
Using the newIV from previous step, you can use it as
the last parameter of the tradeExactAOutput function.
- Function inputs:
 uint256 exactAmountAOut - Amount of options to buy
 uint256 maxAmountBIn - Slippage control
 address owner - Amount of options to buy
 uint256 sigmaInitialGuess - mewIV found in previous step
*/
// Examples below;
optionAMMPool.tradeExactAOutput(
  amountOfOptions,
  maxAmountBIn,
  owner,
  newIV
 );
 
```
{% endtab %}
{% endtabs %}

### 

