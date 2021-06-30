# How To Remove Liquidity

In order to remove liquidity,  you will need to pass as inputs to the contract the percentage of the exposition on the options side and the token side you want to withdraw.

But how are you going to know how much real assets you will get in exchange for that initial exposure? First of all, you can call the function `getRemoveLiquidityAmounts` passing the same percentages. This function will return the withdrawal amounts of options and tokens. If you want to understand deeper the topic about exposure and position, [check this section.](https://app.gitbook.com/@pods-finance-1/s/teste/~/drafts/-MU6ZVfqbnZEcmT18mVa/options-amm-overview/optionamm/components)

{% hint style="info" %}
Keep in mind that the withdrawal amounts between calling the `getRemoveLiquidityAmounts` and `removeLiquidity`may change due to market changes.
{% endhint %}

### Example 1 - Full Liquidity Remove

Imagine you have added some amount of liquidity of `options` and some liquidity of `tokenB.` Now you want to remove 100% of your exposition of both tokens. The steps you should do are: 

1\) Call `getRemoveLiquidityAmounts`  first in order to estimate the withdrawal amounts  
2\) Call `removeLiquidty` on `OptionAMMPool`with the percents that you want to remove your exposition.

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");
uint256 percentA = 100 // if you want to remove 100%
uint256 percentB = 100 // if you want to remove 100%

// You can check with getRemoveLiquidityAmounts to estimate the amount of assets you will remove
(uint256 withdrawAmountA, uint256 withdrawAmountB) = optionAMMPool.getRemoveLiquidityAmounts(percentA, percentB);

optionAMMPool.removeLiquidity(percentA, percentB);

    
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Parameters example
const owner = '0x3ab...';
const optionAMMPoolAddress = '0x3c...'

/////////////
// Web3.js

// Instantiate contract
const optionAMMPool = await web3.eth.Contract('Contract ABI', optionAMMPoolAddress)

const [withdrawAmountOptions, withdrawTokenB] = await optionAMMPool.methods.getRemoveLiquidityAmounts(percentA, percentB).call()

await optionAMMPool.methods.removeLiquidity(percentA, percentB).send({})

/////////////
// Ethers.js

// Instantiate contract
const optionAMMPool = await ethers.getContractAt(optionAMMPoolAddress, 'Contract ABI')

const [withdrawAmountOptions, withdrawTokenB] = await optionAMMPool.getRemoveLiquidityAmounts(percentA, percentB)

await optionAMMPool.removeLiquidity(percentA, percentB)
```
{% endtab %}
{% endtabs %}



