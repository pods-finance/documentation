# How To Create Your Own Option

You will need 4 steps to create your own option:

1. Get the `OptionFactory`address 
2. Get either the `OptionFactory ABI` \(web3\) or the `OptionFactory interface` \(solidity\)
3. Define the parameters of your option
4. Call the `createOption()` function with the parameters

### 1. Get the OptionFactory address

You can find the OptionFactory address using two ways:

1\) Check our deployed contracts page [here](../developers/deployed-contracts.md).

2\) Instantiate our `ConfigurationManager` contract and call the function `getOptionFactory()`.

### 2. Get OptionFactory ABI or Interface

You can find both ABI and interface of OptionFactory in our Github repo. Always check if you are on the master branch to get our latest live update.

* Interfaces [here](https://github.com/pods-finance/contracts/tree/master/contracts/interfaces).
* ABIs [here](https://github.com/pods-finance/contracts/tree/master/abi).

### 3. Define your options parameters

In order to deploy a new option series, we need to define if the option will be a put, or call, expiration date and exercise size window are some of the option properties.

Below you can find all the parameters that you will need to define:

|  Input Name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| name | string | any | The option token name \(e.g., Pod BTC:USDC 300\) |
| symbol | string | any | The option token symbol \(e.g., "PodPutBTC"\) |
| optionType | uint8 | 0 / 1 | 0 for Put, 1 for Call |
| exerciseType | uint8 | 0 / 1 | 0 for American / 1 for European |
| underlyingAsset | address | contract address | Underlying asset address  |
| strikeAsset | address | contract address | Strike asset address. Works with interest-bearing tokens aToken from Aave. |
| strikePrice | uint256 | any | Strike price \(units of strikeAsset that buy 1 unit of underlying\) \(e.g., 12000000000 means you need 12000000000 units of strike asset to exercise 1 unit of WBTC.\)   |
| expiration | uint256 | higher than current block timestamp | Expiration option date in UNIX timestamp \(e.g., 1609401600\) |
| exerciseWindowSize | uint256 | higher than  86400 \(24h\) | Duration of the exercise windows in seconds. |
| isAave | bool |  | If true, deploys a different contract version that supports liquidity mining on Aave \(claim rewards\) |

### 4. Call the `createOption()` function

Now that we have the `OptionFactory` address, ABI, interface and we have also defined the initial parameters, its time call the `createOption()`function.

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate Option Factory
OptionFactory optionFactory = OptionFactory("/*address*/");

// Parameters example
string name = "WBTC:USDC 12000 31Dec 2020";
string symbol = "PodPut WBTC:USDC";
uint8 optionType = 0; // 0 for Put, 1 for Call
uint8 exerciseType = 0; // 0 for American, 1 for European
//WBTC
address underlyingAsset = 0x2260fac5e5542a773aa44fbcfedf7c193bc2c599;
// USDC
address strikeAsset = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 
uint256 strikePrice = 12000000000; // Using 6 decimals, equal to strikeAsset decimals
uint256 expiration = 1609401600; // timestamp in seconds: 31 Dec 2020 08AM
uint256 exerciseWindowSize = 86400; // 24H in seconds
uint256 isAave = false; // if the collateral token is not aTokens


optionFactory.createOption(
        name,
        symbol,
        optionType,
        exerciseType,
        underlyingAsset,
        strikeAsset,
        strikePrice,
        expiration,
        exerciseWindowSize,
        isAave
    )
    
// OptionFactory.sol
/**
     * @notice creates a new PodPut Contract
     * @param _name The option token name. Eg. "Pods Put WBTC-USDC 5000 2020-02-23"
     * @param _symbol The option token symbol. Eg. "podWBTC:20AA"
     * @param _optionType The option type. Eg. "0 for Put, 1 for Call"
     * @param _exerciseType The exercise type. Eg. "0 for European, 1 for American"
     * @param _underlyingAsset The underlying asset. Eg. "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"
     * @param _strikeAsset The strike asset. Eg. "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
     * @param _strikePrice The option strike price including decimals (strikePriceDecimals == strikeAssetDecimals), Eg, 5000000000
     * @param _expiration The Expiration Option date in UNIX timestamp. E.g 1600178324
     * @param _exerciseWindowSize The Expiration Window Size duration in UNIX timestamp. E.g 24*60*60 (24h)
     * @param _isAave True if the collateral token is aToken
 */
    function createOption(
        string memory _name,
        string memory _symbol,
        PodOption.OptionType _optionType,
        PodOption.OptionType _exerciseType,
        address _underlyingAsset,
        address _strikeAsset,
        uint256 _strikePrice,
        uint256 _expiration,
        uint256 _exerciseWindowSize,
        bool isAave
    ) public returns (address) {}
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Parameters example
const optionFactoryAddress = '0x3c...'

const constructParameters = [
"WBTC:USDC 12000 31Dec 2020"; // name
"PodPut WBTC:USDC"; // symbol
0; // OptionType: 0 for Put, 1 for Call
0; // ExerciseType: 0 for American, 1 for European
"0x2260fac5e5542a773aa44fbcfedf7c193bc2c599"; // strikeAsset address
"0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"; // underlyingAsset address
"12000000000"; // StrikePrice: Using 6 decimals, equal to strikeAsset decimals
1609401600; // timestamp in seconds: 31 Dec 2020 08AM
86400; // exercise window size
false; // if the collateral is Aave and you want to have liquidity mining support
]

/////////////
// Web3.js

// Instantiate contract
const optionFactory = await web3.eth.Contract('Contract ABI', optionFactoryAddress)

await optionFactory.methods.createOption(...constructorParameters).send({})

/////////////
// Ethers.js

// Instantiate contract
const optionFactory = await ethers.getContractAt(optionFactoryAddress, 'Contract ABI')

await optionFactory.createOption(...constructorParameters)
```
{% endtab %}
{% endtabs %}

### 

### 



