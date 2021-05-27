# Smart Contracts

![Options AMM contracts diagram](../../.gitbook/assets/options-amm-v4-1-.png)

## Main contracts

### OptionAMMFactory

Contract responsible for creating/deploying new AMM pair pools. It has only the following functions:

* createPool\(\) 
* getPool\(\)

### Configuration Manager

Contract responsible for managing all the addresses for the auxiliary contracts. It contains BlackScholes, IVGuesser, IVProvider, NormalDistribution, and Price Provider setters and getters.

### OptionAMMPool

The contract pool responsible for trade, add and remove liquidity of a pair `PodOption:ERC20`. Some of the actions that you can do:

* addLiquidity\(\) 
* removeLiquidity\(\)
* tradeExactAInput\(\)
* tradeExactAOutput\(\)
* tradeExactBInput\(\)
* tradeExactBOutput\(\)
* getUserBalance\(\)
* getPoolBalances\(\)
* getRemoveLiquidityAmounts\(\)

## Auxiliary Contracts

### BlackScholes

Contract responsible for the calculation of the BlackScholes formula for Puts and Calls. This contract uses the `NormalDistribution` contract to calculate the probability functions. You can check the prices using:

* getCallPrice\(\)
* getPutPrice\(\)

### IVGuesser

To find the IV \(also known as sigma\) for a given price is not trivial. We need to use a numeric method. This contract is responsible for the interaction process that will find the IV given a `targetPrice` depending if the option is a Call or a Put. For gas cost purposes, a variable called `IVInitialGuess` can be passed to help the numeric method converge faster.

* getCallIV\(\)
* getCallPut\(\)

### NormalDistribution

Since we do not have libraries to deal with normal distribution functions, and our goal is to be as cost-efficient as possible, we can use  this contract to find probabilities given a Z using:

* getProbability\(\)

### IVProvider

Contract responsible for finding the `OracleIV` of a given option. That IV will represent the external market IV of a given option, taking into account the option strike price, maturity, and underlying asset. As of now, the oracle is updated by the team \(admin keys\) since the current solutions for IV oracles are nascent. The weight of the Oracle IV can be reduced in big liquidity scenarios or increased, in low liquidity scenarios. We looking forward to exploring TWAP solutions in the future. This way users would need to do even more large transactions to manipulate the Pool's IV. 

* getIV\(\)
* updateIV\(\)
* setUpdater\(\)

### PriceProvider

Contract responsible for finding the `spotPrice` of a given asset that the `priceFeed` was previously registered. This contract can interact with many different price sources and even create an average between feeds. You can use it calling:

* getPriceFeed\(\)
* latestRoundData\(\)
* getAssetPrice\(\)
* getAssetDecimals\(\)
* setAssetFeeds\(\)
* removeAssetFeeds\(\)

### PriceProvider

Contract responsible for finding the `spotPrice` of a given asset that the `priceFeed` was previously registered. This contract can interact with many different price sources and even create an average between feeds. You can use it calling:

* getPriceFeed\(\)
* latestRoundData\(\)
* getAssetPrice\(\)
* getAssetDecimals\(\)
* setAssetFeeds\(\)
* removeAssetFeeds\(\)

### PriceProvider

Contract responsible for finding the `spotPrice` of a given asset that the `priceFeed` was previously registered. This contract can interact with many different price sources and even create an average between feeds. You can use it calling:

* getPriceFeed\(\)
* latestRoundData\(\)
* getAssetPrice\(\)
* getAssetDecimals\(\)
* setAssetFeeds\(\)
* removeAssetFeeds\(\)

### ChainlinkPriceFeed

Its our facade Chainlink contract used to get `spotPrice` and even other data in future like implied volatility:

* getLatestPrice\(\)
* latestRoundData\(\)
* decimals\(\)

### FeePool

Contract responsible for managing the fees paid to the LP providers. This contract is not supposed to be called from an EOA \(external owned account\). We are showing here only to make a more digestible understanding of the whole system.

### Emergency Stop

Contract responsible for pause and resume a certain target contract.

