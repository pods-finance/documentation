# OptionAMMFactory

Contract responsible for creating/deploying new `OptionAMMpools`. This contract is supposed to be used along with Pods financial instrument `PodOption.` If you want to implement the Pool yourself, talk to us on our Discord channel and we will give you assistance.

## Methods

### createPool

This function is meant to be called by a caller who wants to deploy a new instance of a Pool contract.

Returns a new instance of OptionAMMPool.

| input name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| \_optionAddress | address | PodOption contracts | The address of the option token \(PodOption\) |
| \_stableAsset | address | ERC20 token | The ERC20 will be used as the pair of the option. Will be easier if the price of the option is denominated in any stablecoin. |
| \_priceProvider | address | - | Contract address of the PriceProvider contract for spotPrice |
| \_priceMethod | address | - | Contract address of the PriceMethod contract \(E.g: BlackScholes\) |
| \_sigma | address | - | Contract address of the sigma \(Implied Volatility - IV\) |
| \_initialSigma | uint256 |  | The Initial number of sigma \(Implied Volatility\) with 18 decimals |

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate OptionAMMFactory
OptionAMMFactory optionAMMFactory = OptionAMMFactory("/*address*/");

// Parameters example
// You can check our deployed contracts of priceProvider, priceMethod and Sigma
address optionAddress = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 
address stableAsset = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 
address priceProvider = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 
address priceMethod = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 
address sigma = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48;

address initialSigma = '660000000000000000;  // 66% with 18 decimals



optionAMMFactory.createPool(
        optionAddress,
        stableAsset,
        priceProvider,
        priceMethod,
        sigma,
        initialSigma
    );

// OptionAMMFactory.sol
    /**
     * @notice Creates an option pool
     *
     * @param _optionAddress The address of option token
     * @param _stableAsset A stablecoin asset address
     * @param _priceProvider contract address of the PriceProvider contract for spotPrice
     * @param _priceMethod contract address of the PriceMethod contract (E.g: BlackScholes)
     * @param _sigma contract address of the sigma (implied Volatility) contract
     * @param _initialSigma Initial number of sigma (implied volatility)
     * @return The address of the newly created pool
     */
    function createPool(
        address _optionAddress,
        address _stableAsset,
        address _priceProvider,
        address _priceMethod,
        address _sigma,
        uint256 _initialSigma
    ) external override returns (address OptionAMMPool) {}
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Parameters example
// You can check our deployed contracts of priceProvider, priceMethod and Sigma
const optionAddress = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 
const stableAsset = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 
const priceProvider = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 
const priceMethod = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 
const sigma = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48;

const initialSigma = '660000000000000000;  // 66% with 18 decimals

const optionAMMFactoryAddress = '0x3c...'

/////////////
// Web3.js

// Instantiate contract
const optionAMMFactory = await web3.eth.Contract('Contract ABI', optionAMMFactoryAddress)

await optionAMMFactory.methods.createPool(
    optionAddress, 
    stableAsset, 
    priceProvider, 
    priceMethod, 
    sigma,
    initialSigma
).send({ from: 'yourWallet' })

/////////////
// Ethers.js

// Instantiate contract
const optionAMMFactory = await ethers.getContractAt(optionAMMFactoryAddress, 'Contract ABI')

await optionAMMFactory.createPool(
    optionAddress, 
    stableAsset, 
    priceProvider, 
    priceMethod, 
    sigma,
    initialSigma
)
```
{% endtab %}
{% endtabs %}

### getPool

The function returns the address of the pool with given an `_optionAddress.`

| Input name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| \_optionAddress | address | - | The address of the option token \(PodOption\) |

{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate OptionAMMFactory
OptionAMMFactory optionAMMFactory = OptionAMMFactory("/*address*/");

address optionAddress = '0xe3..."

optionAMMFactory.getPool(optionAddress)

// OptionAMMFactory.sol
     /**
     * @notice Returns the address of a previously created pool
     *
     * @dev If the pool has not been created it will return address(0)
     *
     * @param _optionAddress The address of option token
     * @return The address of the pool
     */
    function getPool(
        address _optionAddress
    ) external returns (address pool) {}
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Parameters example
const optionAddress = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48; 

/////////////
// Web3.js

// Instantiate contract
const optionAMMFactory = await web3.eth.Contract('Contract ABI', optionAMMFactoryAddress)

const poolAddress = await optionAMMFactory.methods.getPool(optionAddress).call()

/////////////
// Ethers.js

// Instantiate contract
const optionAMMFactory = await ethers.getContractAt(optionAMMFactoryAddress, 'Contract ABI')

const poolAddress = await optionAMMFactory.getPool(optionAddress)
```
{% endtab %}
{% endtabs %}

