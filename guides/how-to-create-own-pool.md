# How To Create Your Own Pool

You will need 4 steps to create your own option:

1. Get the `OptionAMMFactory`address&#x20;
2. Get either the `OptionAMMFactory ABI` (web3) or the `OptionAAMMFactory interface` (solidity)
3. Define the parameters of your pool
4. Call the `createPool()` function with the parameters

### 1. Get the OptionAMMFactory address

1\) Instantiate our `ConfigurationManager` contract. Check our deployed contracts page [here](../developers/deployed-contracts.md).

2\) Call the function `getAMMFactory()`.

### 2. Get OptionAMMFactory ABI or Interface

You can find both ABI and interface of OptionFactory in our Github repo. Always check if you are on the master branch to get our latest live update.

* Interfaces [here](https://github.com/pods-finance/contracts/tree/master/contracts/interfaces).
* ABIs [here](https://github.com/pods-finance/contracts/tree/master/abi).

### 3. Define your pool parameters

In order to deploy a new option amm pool you will need only 3 parameters:&#x20;

* OptionTokenAddress
* StableTokenAddress (We use the assumption that any stable token is equal to 1 USD)
* InitialIV => This is the initial implied volatility for the pool

### 4. Call the `createPool()` function

Now that we have the `OptionAMMFactory` address, ABI, interface and we have also defined the initial parameters, its time call the `createPool()`function.

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option Factory
OptionAMMFactory optionAMMFactory = OptionAMMFactory("/*address*/");

address optionAddress = 0x2260fac5e5542a773aa44fbcfedf7c193bc2c599;
address stableAsset = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 
uint256 initialIV = 1200000000000000000; // Using 18 decimals (120%)

optionAMMFactory.createPool(
        optionAddress,
        stableAsset,
        initialIV
    )
    
// OptionAMMFactory.sol
/**
     * @param _optionAddress The option address (created with our OptionFactory).
     * @param _stableAsset The stable token address. 
     * @param _initialIV The 
*/
    function createPool(
        address _optionAddress,
        address _stableAsset,
        uint256 _initialIV
    ) public returns (address) {}
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Parameters example
const optionAMMFactoryAddress = '0x3c...'

const constructorParameters = [
"0x2260fac5e5542a773aa44fbcfedf7c193bc2c599", // option address
"0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48", // stable asset address
"1200000000000000000" // InitialIV: Using 18 decimals (E.g: 120%)
]

/////////////
// Web3.js

// Instantiate contract
const optionAMMFactory = await web3.eth.Contract('Contract ABI', optionAMMFactoryAddress)

await optionAMMFactory.methods.createPool(...constructorParameters).send({})

/////////////
// Ethers.js

// Instantiate contract
const optionAMMFactory = await ethers.getContractAt(optionAMMFactoryAddress, 'Contract ABI')

await optionAMMFactory.createPool(...constructorParameters)
```
{% endtab %}
{% endtabs %}

###
