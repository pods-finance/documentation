# OptionAMMPool

The contract pool responsible for trade, add and remove liquidity of a pair `PodOption:ERC20`.

## Properties

<table>
  <thead>
    <tr>
      <th style="text-align:left">input name</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Required</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">tokenA</td>
      <td style="text-align:left">address</td>
      <td style="text-align:left">PodOption contracts</td>
      <td style="text-align:left">Must be an option token (PodOption)</td>
    </tr>
    <tr>
      <td style="text-align:left">tokenB</td>
      <td style="text-align:left">address</td>
      <td style="text-align:left">ERC20 token</td>
      <td style="text-align:left">The ERC20 will be used as the pair of the option. Will be easier if the
        price of the option is denominated in any stablecoin.</td>
    </tr>
    <tr>
      <td style="text-align:left">priceProvider</td>
      <td style="text-align:left">address</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">Contract address of the PriceProvider contract for spotPrice</td>
    </tr>
    <tr>
      <td style="text-align:left">priceMethod</td>
      <td style="text-align:left">address</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">Contract address of the PriceMethod contract (E.g: BlackScholes)</td>
    </tr>
    <tr>
      <td style="text-align:left">impliedVolatility</td>
      <td style="text-align:left">address</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">Contract address of the sigma (Implied Volatility - IV)</td>
    </tr>
    <tr>
      <td style="text-align:left">feePoolA</td>
      <td style="text-align:left">address</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Contract address of the fee pool related to the tokenA</td>
    </tr>
    <tr>
      <td style="text-align:left">feePoolB</td>
      <td style="text-align:left">address</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Contract address of the fee pool related to the tokenB</td>
    </tr>
    <tr>
      <td style="text-align:left">priceProperties</td>
      <td style="text-align:left">struct</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>All the properties related to the pricing calculation:</p>
        <ul>
          <li>option expiration</li>
          <li>option strikePrice</li>
          <li>option underlyingAsset</li>
          <li>option type</li>
          <li>pool current sigma (IV)</li>
          <li>pool current risk-free rate</li>
          <li>pool last sigma initial guess</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Methods

### addLiquidity

This function is used to add new liquidity to the pool. Since we use a single-sided AMM, `amountOfA` or `amountOfB` can be 0 if you want to add liquidity to just one side of the pool. This function can only be called before option expiration.

|  input name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| amountOfA | uint256 | - | Amount of Token A to add |
| amountOfB | uint256 | - | Amount of Token B to add |
| owner | address | - | Add on behalf of someone  |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
uint256 amountOfA = 10000000; // Need to take in consideration asset decimals
uint256 amountOfB = 10000000000000; // Need to take in consideration asset decimals
address owner = '0x3ab...';

optionAMMPool.addLiquidity(amountOfA, amountOfB, owner);
    
// OptionAMMPool.sol
    /**
     * @notice addLiquidity in any proportion of tokenA or tokenB
     *
     * @dev This function can only be called before option expiration
     *
     * @param amountOfA amount of TokenA to add
     * @param amountOfB amount of TokenB to add
     * @param owner address of the account that will have ownership of the liquidity
     */
    function addLiquidity(
        uint256 amountOfA,
        uint256 amountOfB,
        address owner
    ) external override beforeExpiration {}
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Parameters example
// You can check our deployed contracts of priceProvider, priceMethod and Sigma
const amountOfA = 10000000; // Need to take in consideration asset decimals
const amountOfB = 10000000000000; // Need to take in consideration asset decimals
const owner = '0x3ab...';

const optionAMMPoolAddress = '0x3c...'

/////////////
// Web3.js

// Instantiate contract
const optionAMMPool = await web3.eth.Contract('Contract ABI', optionAMMPoolAddress)

await optionAMMPool.methods.addLiquidity(
    amountOfA, 
    amountOfB, 
    owner
).send({ from: 'yourWallet' })

/////////////
// Ethers.js

// Instantiate contract
const optionAMMPool = await ethers.getContractAt(optionAMMPoolAddress, 'Contract ABI')

await optionAMMPool.addLiquidity(
    amountOfA, 
    amountOfB, 
    owner
)
```
{% endtab %}
{% endtabs %}

### removeLiquidity

This function is used to remove liquidity from the pool. Since we use a single-sided AMM, `percentA` or `percentB` can be 0 if you want to remove liquidity from just one side of the pool. The percentA and percentB represent the percentage of the exposition in each asset you want to remove. If you want to check upfront what the amount represent certain exposition, you can call `getRemoveLiquidityAmounts.`

|  input name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| percentA | uint256 | 0 - 100 | Percent of the exposition of amount of Token A to remove \(Eg: 100 = 100% available\) |
| percentB | uint256 | 0 - 100 | Exposition of amount of Token B to remove |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

uint256 percentA = 100; // Will remove 100% of exposition of Asset A
uint256 percentB = 50; // Will remove 50% of exposition of Asset B

optionAMMPool.removeLiquidity(percentA, percentB);
    
// OptionAMMPool.sol
  /**
     * @notice removeLiquidity in any proportion of tokenA or tokenB
     *
     * @param amountOfA amount of TokenA to add
     * @param amountOfB amount of TokenB to add
     */
    function removeLiquidity(
    uint256 percentA, 
    uint256 percentB
    ) external override {}
```
{% endtab %}

{% tab title="Web3" %}
```javascript
// Parameters example
// You can check our deployed contracts of priceProvider, priceMethod and Sigma
const amountOfA = 10000000; // Need to take in consideration asset decimals
const amountOfB = 10000000000000; // Need to take in consideration asset decimals

const optionAMMPoolAddress = '0x3c...'

/////////////
// Web3.js

// Instantiate contract
const optionAMMPool = await web3.eth.Contract('Contract ABI', optionAMMPoolAddress)

await optionAMMPool.methods.removeLiquidity(
    amountOfA, 
    amountOfB
).send({ from: 'yourWallet' })

/////////////
// Ethers.js

// Instantiate contract
const optionAMMPool = await ethers.getContractAt(optionAMMPoolAddress, 'Contract ABI')

await optionAMMPool.removeLiquidity(
    amountOfA, 
    amountOfB
)
```
{% endtab %}
{% endtabs %}

### tradeExactAInput

This function represents "selling token A to the pool" or "buying token B from the pool". `msg.sender` is able to trade exact amount of token A in exchange for minimum amount of token B and send the tokens B to the `owner`. After that, this function also updates the `priceProperties.currentSigma.`

|  input name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| exactAmountAIn | uint256 | - | Exact amount of A token that will be transfer from `msg.sender` |
| minAmountBOut | uint256 | - | Minimum acceptable amount of token B to transfer to `owner` |
| owner | address |  | address that will receive the token in exchange |
| sigmaInitialGuess | uint256 | != 0 | For gas cost-saving purpose, it is possible to run getOptinTradedDDetails before the trade in order to send the best sigma possible |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
 
uint256 exactAmountAIn = 10000000; // Need to take in consideration asset decimals
uint256 minAmountBOut = 10000000000000; // Need to take in consideration asset decimals
address owner = 0x3a...
uint256 sigmaInitialGuess = 660000000000 // should run getOptionTradeDetails before 

optionAMMPool.tradeExactAInput(
    exactAmountAIn, 
    minAmountBOut, 
    owner,
    sigmaInitialGuess
    );
  
// IOptionAMMPool.sol  
/**
     * @notice tradeExactAInput msg.sender is able to trade exact amount of token A in exchange for minimum
     * amount of token B and send the tokens B to the owner. After that, this function also updates the
     * priceProperties.currentSigma
     *
     * @dev sigmaInitialGuess is a parameter for gas saving costs purpose. Instead of calculating the new sigma
     * out of thin ar, caller can help the Numeric Method achieve the result in less iterations with this parameter.
     * In order to know which guess the caller should use, call the getOptionTradeDetailsExactAInput first.
     *
     * @param exactAmountAIn exact amount of A token that will be transfer from msg.sender
     * @param minAmountBOut minimum acceptable amount of token B to transfer to owner
     * @param owner the destination address that will receive the token B
     * @param sigmaInitialGuess The first guess that the Numeric Method (getPutSigma / getCallSigma) should use
     */
    function tradeExactAInput(
        uint256 exactAmountAIn,
        uint256 minAmountBOut,
        address owner,
        uint256 sigmaInitialGuess
    ) external override beforeExpiration returns (uint256) {}
```
{% endtab %}
{% endtabs %}

### tradeExactAOutput

This function represents "buying token A from the pool" or "selling token B to the pool". `owner` is able to receive exact amount of token A in exchange of a max acceptable amount of token B transfer from the `msg.sender`. After that, this function also updates the `priceProperties.currentSigma`

|  input name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| exactAmountAOut | uint256 | - | Exact amount of token A that will be transfer to `owner` |
| maxAmountBIn | uint256 | - | Maximum acceptable amount of token B to transfer from `msg.sender` |
| owner | address |  | address that will receive the token in exchange |
| sigmaInitialGuess | uint256 | != 0 | For gas cost-saving purpose, it is possible to run getOptionTradedDDetails before the trade in order to send the best sigma possible |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
 
uint256 exactAmountAOut = 10000000; // Need to take in consideration asset decimals
uint256 maxAmountBIn = 10000000000000; // Need to take in consideration asset decimals
address owner = 0x3a...
uint256 sigmaInitialGuess = 660000000000 // should run getOptionTradeDetails before 

optionAMMPool.tradeExactAOutput(
    exactAmountAOut, 
    maxAmountBIn, 
    owner,
    sigmaInitialGuess
    );
    
// IOptionAMMPool.sol
/**
     * @notice _tradeExactAOutput owner is able to receive exact amount of token A in exchange of a max
     * acceptable amount of token B transfer from the msg.sender. After that, this function also updates
     * the priceProperties.* currentSigma
     *
     * @dev sigmaInitialGuess is a parameter for gas saving costs purpose. Instead of calculating the new sigma
     * out of thin ar, caller can help the Numeric Method achieve the result in less iterations with this parameter.
     * In order to know which guess the caller should use, call the getOptionTradeDetailsExactAOutput first.
     *
     * @param exactAmountAOut exact amount of token A that will be transfer to owner
     * @param maxAmountBIn maximum acceptable amount of token B to transfer from msg.sender
     * @param owner the destination address that will receive the token A
     * @param sigmaInitialGuess The first guess that the Numeric Method (getPutSigma / getCallSigma) should use
     */
    function tradeExactAOutput(
        uint256 exactAmountAOut,
        uint256 maxAmountBIn,
        address owner,
        uint256 sigmaInitialGuess
    ) external returns (uint256)';
```
{% endtab %}
{% endtabs %}

### tradeExactBInput

This function represents "selling token B to the pool" or "buying token A from the pool". `msg.sender` is able to trade exact amount of token B in exchange for minimum amount of token A and send the tokens B to the `owner`. After that, this function also updates the `priceProperties.currentSigma.`

|  input name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| exactAmountBIn | uint256 | - | Exact amount of A token that will be transfer from `msg.sender` |
| minAmountAOut | uint256 | - | Minimum acceptable amount of token B to transfer to `owner` |
| owner | address |  | address that will receive the token in exchange |
| sigmaInitialGuess | uint256 | != 0 | For gas cost-saving purpose, it is possible to run getOptionTradedDDetails before the trade in order to send the best sigma possible |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
uint256 exactAmountBIn = 10000000; // Need to take in consideration asset decimals
uint256 minAmountAOut = 10000000000000; // Need to take in consideration asset decimals
address owner = 0x3a...
uint256 sigmaInitialGuess = 660000000000 // should run getOptionTradeDetails before 

optionAMMPool.tradeExactBInput(
    exactAmountBIn, 
    minAmountAOut, 
    owner,
    sigmaInitialGuess
    );
    
// IOptionAMMPool.sol
/**
     * @notice _tradeExactBInput msg.sender is able to trade exact amount of token B in exchange for minimum
     * amount of token A sent to the owner. After that, this function also updates the priceProperties.currentSigma
     *
     * @dev sigmaInitialGuess is a parameter for gas saving costs purpose. Instead of calculating the new sigma
     * out of thin ar, caller can help the Numeric Method achieve the result ini less iterations with this parameter.
     * In order to know which guess the caller should use, call the getOptionTradeDetailsExactBInput first.
     *
     * @param exactAmountBIn exact amount of token B that will be transfer from msg.sender
     * @param minAmountAOut minimum acceptable amount of token A to transfer to owner
     * @param owner the destination address that will receive the token A
     * @param sigmaInitialGuess The first guess that the Numeric Method (getPutSigma / getCallSigma) should use
     */
    function tradeExactBInput(
        uint256 exactAmountBIn,
        uint256 minAmountAOut,
        address owner,
        uint256 sigmaInitialGuess
    ) external returns (uint256);
```
{% endtab %}
{% endtabs %}

### tradeExactBOutput

This function represents "selling token A to the pool" or "buying token B from the pool". `owner` is able to receive exact amount of token B in exchange of a max acceptable amount of token A transfer from the `msg.sender`. After that, this function also updates the `priceProperties.currentSigma`

|  input name | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| exactAmountBOut | uint256 | - | Exact amount of token B that will be transfer to `owner` |
| maxAmountAIn | uint256 | - | Maximum acceptable amount of token A to transfer from `msg.sender` |
| owner | address |  | address that will receive the token in exchange |
| sigmaInitialGuess | uint256 | != 0 | For gas cost-saving purpose, it is possible to run getOptionTradedDDetails before the trade in order to send the best sigma possible |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
 
uint256 maxAmountBIn = 10000000; // Need to take in consideration asset decimals
uint256 exactAmountBOut = 10000000000000; // Need to take in consideration asset decimals
address owner = 0x3a...
uint256 sigmaInitialGuess = 660000000000 // should run getOptionTradeDetails before 

optionAMMPool.tradeExactBOutput(
    exactAmountBOut, 
    maxAmountBIn, 
    owner,
    sigmaInitialGuess
    );
    
// IOptionAMMPool.sol
/**
     * @notice _tradeExactAOutput owner is able to receive exact amount of token A in exchange of a max
     * acceptable amount of token B transfer from the msg.sender. After that, this function also updates
     * the priceProperties.* currentSigma
     *
     * @dev sigmaInitialGuess is a parameter for gas saving costs purpose. Instead of calculating the new sigma
     * out of thin ar, caller can help the Numeric Method achieve the result in less iterations with this parameter.
     * In order to know which guess the caller should use, call the getOptionTradeDetailsExactAOutput first.
     *
     * @param exactAmountAOut exact amount of token A that will be transfer to owner
     * @param maxAmountBIn maximum acceptable amount of token B to transfer from msg.sender
     * @param owner the destination address that will receive the token A
     * @param sigmaInitialGuess The first guess that the Numeric Method (getPutSigma / getCallSigma) should use
     */
    function tradeExactBOutput(
        uint256 exactAmountBOut,
        uint256 maxAmountAIn,
        address owner,
        uint256 sigmaInitialGuess
    ) external returns (uint256);
```
{% endtab %}
{% endtabs %}

### getPoolBalances

Check the current pool amount of Token A and Token B.

#### Return Values

| Input name | Type | Description |
| :--- | :--- | :--- |
| `totalTokenA` | uint256 | Pool total balance of token A |
| `totalTokenB` | uint256 | Pool total balance of token B |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
(uint256 totalTokenA, uint256 totalTokenB) = optionAMMPool.getPoolBalances();
    
// IOptionAMMPool.sol
/**
     * @notice getPoolBalances external function that returns the current pool balance of token A and token B
     *
     * @return totalTokenA balanceOf this contract of token A
     * @return totalTokenB balanceOf this contract of token B
     */
    function getPoolBalances() external view returns (uint256 totalTokenA, uint256 totalTokenB)
```
{% endtab %}
{% endtabs %}

### getUserDepositSnapshot

Check the original balance of a user, returning its original deposit of Token A, the original balance of Token B, and the opening value factor at the moment of the deposit.

#### Input Parameters

| Input name | Type | Description |
| :--- | :--- | :--- |
| `user` | uint256 | Address to consult the balance |

#### Return Values

| Input name | Type | Description |
| :--- | :--- | :--- |
| `tokenABalance` | uint256 | Exact amount of token B that will be transfer to `owner` |
| `tokenBBalance` | uint256 | Maximum acceptable amount of token A to transfer from `msg.sender` |
| `fImpUser` | uint256 | address that will receive the token in exchange |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
address user = '0x3aab...';

(uint256 tokenABalance, uint256 tokenBBalance, uint256 fImpUser) = 
optionAMMPool.getUserDepositSnapshot(user);
    
// IOptionAMMPool.sol
/**
 * @notice getUserDepositSnapshot external function that User original balance of token A,
 * token B and the Opening Value * * Factor (Fimp) at the moment of the liquidity added
 *
 * @param user address to check the balance info
 *
 * @return tokenABalance balance of token A by the moment of deposit
 * @return tokenBBalance balance of token B by the moment of deposit
 * @return fImpUser value of the Opening Value Factor by the moment of the deposit
*/
function getUserDepositSnapshot(address user) 
 external 
 view 
 returns ( uint256 tokenABalance, uint256 tokenBBalance, uint256 fImpUser );
```
{% endtab %}
{% endtabs %}

### getOptionTradeDetailsExactAInput

View function that simulates a trade, in order the preview `amountBOut`, the new sigma \(IV\), that will be used as the `sigmaInitialGuess` if the caller wants to perform a trade in the sequence. Also returns the amount of Fees that will be paid to liquidity pools A and B.

#### Input Parameters

| Input name | Type | Description |
| :--- | :--- | :--- |
| `exactAmountAIn` | uint256 | amount of token A that will be transfer from msg.sender to the pool |

#### Return Values

| Input name | Type | Description |
| :--- | :--- | :--- |
| `amountBOut` | uint256 | amount of B in exchange of the `exactAmountAIn` |
| `newIV` | uint256 | New sigma that this trade will result |
| `feesTokenA` | uint256 | Amount of fees of collected by token A |
| `feesTokenB` | uint256 | amount of fees of collected by token B |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
address exactAmountAIn = 100000;

(uint256 amountBOut, uint256 newIV, uint256 feesTokenA, uint256 feesTokenB) = 
optionAMMPool.getOptionTradeDetailsExactAInput(exactAmountAIn);
    
// IOptionAMMPool.sol
 /**
     * @notice getOptionTradeDetailsExactAInput view function that simulates a trade, in order the preview
     * the amountBOut, the new sigma (IV), that will be used as the sigmaInitialGuess if caller wants to perform
     * a trade in sequence. Also returns the amount of Fees that will be payed to liquidity pools A and B.
     *
     * @param exactAmountAIn amount of token A that will by transfer from msg.sender to the pool
     *
     * @return amountBOut amount of B in exchange of the exactAmountAIn
     * @return newIV the new sigma that this trade will result
     * @return feesTokenA amount of fees of collected by token A
     * @return feesTokenB amount of fees of collected by token B
     */
    function getOptionTradeDetailsExactAInput(uint256 exactAmountAIn)
        external
        override
        view
        returns (
            uint256 amountBOut,
            uint256 newIV,
            uint256 feesTokenA,
            uint256 feesTokenB
        );
```
{% endtab %}
{% endtabs %}

### getOptionTradeDetailsExactAOutput

View function that simulates a trade, in order the preview the `amountBIn`, the new sigma \(IV\), that will be used as the `sigmaInitialGuess` if the caller wants to perform a trade in the sequence. Also returns the amount of Fees that will be paid to liquidity pools A and B.

#### Input Parameters

| Input name | Type | Description |
| :--- | :--- | :--- |
| `exactAmountAOut` | uint256 | amount of token A that will be transfer from the pool to the `owner` |

#### Return Values

| Input name | Type | Description |
| :--- | :--- | :--- |
| `amountBIn` | uint256 | amount of B that will be transfer from `msg.sender` to the pool |
| `newIV` | uint256 | New sigma that this trade will result |
| `feesTokenA` | uint256 | Amount of fees of collected by token A |
| `feesTokenB` | uint256 | amount of fees of collected by token B |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
address exactAmountAOut = 100000;

(uint256 amountBIn, uint256 newIV, uint256 feesTokenA, uint256 feesTokenB) = 
optionAMMPool.getOptionTradeDetailsExactAInput(exactAmountAOut);
    
// IOptionAMMPool.sol
/**
     * @notice getOptionTradeDetailsExactAOutput view function that simulates a trade, in order the preview
     * the amountBIn, the new sigma (IV), that will be used as the sigmaInitialGuess if caller wants to perform
     * a trade in sequence. Also returns the amount of Fees that will be payed to liquidity pools A and B.
     *
     * @param exactAmountAOut amount of token A that will by transfer from pool to the msg.sender/owner
     *
     * @return amountBIn amount of B that will be transfer from msg.sender to the pool
     * @return newIV the new sigma that this trade will result
     * @return feesTokenA amount of fees of collected by token A
     * @return feesTokenB amount of fees of collected by token B
     */
    function getOptionTradeDetailsExactAOutput(uint256 exactAmountAOut)
        external
        override
        view
        returns (
            uint256 amountBIn,
            uint256 newIV,
            uint256 feesTokenA,
            uint256 feesTokenB
        );
```
{% endtab %}
{% endtabs %}

### getOptionTradeDetailsExactBInput

View function that simulates a trade, in order the preview the `amountAOut`, the new sigma \(IV\), that will be used as the `sigmaInitialGuess` if the caller wants to perform a trade in the sequence. Also returns the amount of Fees that will be paid to liquidity pools A and B.

#### Input Parameters

| Input name | Type | Description |
| :--- | :--- | :--- |
| `exactAmountBIn` | uint256 | amount of token B that will be transfer from msg.sender to the pool |

#### Return Values

| Input name | Type | Description |
| :--- | :--- | :--- |
| `amountAOut` | uint256 | amount of A in exchange of the `exactAmountAIn` |
| `newIV` | uint256 | New sigma that this trade will result |
| `feesTokenA` | uint256 | Amount of fees of collected by token A |
| `feesTokenB` | uint256 | amount of fees of collected by token B |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
address exactAmountBIn = 100000;

(uint256 amountAOut, uint256 newIV, uint256 feesTokenA, uint256 feesTokenB) = 
optionAMMPool.getOptionTradeDetailsExactAInput(exactAmountBIn);
    
// IOptionAMMPool.sol
 /**
     * @notice getOptionTradeDetailsExactBInput view function that simulates a trade, in order the preview
     * the amountAOut, the new sigma (IV), that will be used as the sigmaInitialGuess if caller wants to perform
     * a trade in sequence. Also returns the amount of Fees that will be payed to liquidity pools A and B.
     *
     * @param exactAmountBIn amount of token B that will by transfer from msg.sender to the pool
     *
     * @return amountAOut amount of A in exchange of the exactAmountAIn
     * @return newIV the new sigma that this trade will result
     * @return feesTokenA amount of fees of collected by token A
     * @return feesTokenB amount of fees of collected by token B
     */
    function getOptionTradeDetailsExactBInput(uint256 exactAmountBIn)
        external
        override
        view
        returns (
            uint256 amountAOut,
            uint256 newIV,
            uint256 feesTokenA,
            uint256 feesTokenB
        );
```
{% endtab %}
{% endtabs %}

### getOptionTradeDetailsExactBOutput

View function that simulates a trade, in order the preview the `amountAIn`, the new sigma \(IV\), that will be used as the `sigmaInitialGuess` if the caller wants to perform a trade in the sequence. Also returns the amount of Fees that will be paid to liquidity pools A and B.

#### Input Parameters

| Input name | Type | Description |
| :--- | :--- | :--- |
| `exactAmountBOut` | uint256 | amount of token B that will be transfer from the pool to the `owner` |

#### Return Values

| Input name | Type | Description |
| :--- | :--- | :--- |
| `amountAIn` | uint256 | amount of A that will be transfer from `msg.sender` to the pool |
| `newIV` | uint256 | New sigma that this trade will result |
| `feesTokenA` | uint256 | Amount of fees of collected by token A |
| `feesTokenB` | uint256 | amount of fees of collected by token B |

{% tabs %}
{% tab title="Solidity" %}
```javascript
// Instantiate OptionAMMPool
OptionAMMPool optionAMMPool = optionAMMPool("/*address*/");

// Parameters example
address exactAmountBOut = 100000;

(uint256 amountAIn, uint256 newIV, uint256 feesTokenA, uint256 feesTokenB) = 
optionAMMPool.getOptionTradeDetailsExactAInput(exactAmountBOut);
    
// IOptionAMMPool.sol
/**
     * @notice getOptionTradeDetailsExactBOutput view function that simulates a trade, in order the preview
     * the amountAIn, the new sigma (IV), that will be used as the sigmaInitialGuess if caller wants to perform
     * a trade in sequence. Also returns the amount of Fees that will be payed to liquidity pools A and B.
     *
     * @param exactAmountBOut amount of token B that will by transfer from pool to the msg.sender/owner
     *
     * @return amountAIn amount of B that will be transfer from msg.sender to the pool
     * @return newIV the new sigma that this trade will result
     * @return feesTokenA amount of fees of collected by token A
     * @return feesTokenB amount of fees of collected by token B
     */
    function getOptionTradeDetailsExactBOutput(uint256 exactAmountBOut)
        external
        override
        view
        returns (
            uint256 amountAIn,
            uint256 newIV,
            uint256 feesTokenA,
            uint256 feesTokenB
        );
```
{% endtab %}
{% endtabs %}

