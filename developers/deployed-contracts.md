# Deployed Contracts

Pods Protocol is deployed to Ethereum, Polygon, and Arbitrum. Our system does complicated calculations like BlackScholes that by default uses more gas and computational power, so the system works better on scalability solutions.  The following tables contain the details of the contracts on each of the networks.

If you need development support, join the #developers channel on our [Pods community Discord server.](https://discord.gg/Qf7utym)

{% tabs %}
{% tab title="Mainnet" %}
| Contracts            | ABI  | Address                                    |
| -------------------- | ---- | ------------------------------------------ |
| ConfigurationManager | JSON | 0xE4Da64757b2B29dB43429A52CaF7aD884c76f8b0 |
| OptionFactory        | JSON | Check ConfigurationManager                 |
| NormalDistribution   | JSON | Check BlackScholes                         |
| BlackScholes         | JSON | Check ConfigurationManager                 |
| OptionAMMFactory     | JSON | Check ConfigurationManager                 |
| OptionHelper         | JSON | Check ConfigurationManager                 |
{% endtab %}

{% tab title="Polygon" %}
| Contracts            | ABI  | Address                                    |
| -------------------- | ---- | ------------------------------------------ |
| ConfigurationManager | JSON | 0x3294027E4849B1b3155f8B0477bFA37994BB322f |
| OptionFactory        | JSON | Check ConfigurationManager                 |
| NormalDistribution   | JSON | Check BlackScholes                         |
| BlackScholes         | JSON | Check ConfigurationManager                 |
| OptionAMMFactory     | JSON | Check ConfigurationManager                 |
| OptionHelper         | JSON | Check ConfigurationManager                 |
{% endtab %}

{% tab title="Arbitrum" %}


| Contracts            | ABI  | Address                                    |
| -------------------- | ---- | ------------------------------------------ |
| ConfigurationManager | JSON | 0x84601612702C7699c09bBF3C033747709F529008 |
| OptionFactory        | JSON | Check ConfigurationManager                 |
| NormalDistribution   | JSON | Check BlackScholes                         |
| BlackScholes         | JSON | Check ConfigurationManager                 |
| OptionAMMFactory     | JSON | Check ConfigurationManager                 |
| OptionHelper         | JSON | Check ConfigurationManager                 |
{% endtab %}

{% tab title="Kovan" %}
| Contracts            | ABI  | Address                                    |
| -------------------- | ---- | ------------------------------------------ |
| ConfigurationManager | JSON | 0x11dB0eDFe06Cd06C9AFE96b9813043e0853e7926 |
| OptionFactory        | JSON | Check ConfigurationManager                 |
| NormalDistribution   | JSON | Check BlackScholes                         |
| BlackScholes         | JSON | Check ConfigurationManager                 |
| OptionAMMFactory     | JSON | Check ConfigurationManager                 |
| OptionHelper         | JSON | Check ConfigurationManager                 |
{% endtab %}
{% endtabs %}

## Option Series

Separate list only with the options that we have deployed during the last year.

{% hint style="info" %}
If you want the address of any option that is not on the list, talk to us on our Discord.&#x20;
{% endhint %}

{% tabs %}
{% tab title="Mainnet" %}

{% endtab %}

{% tab title="Polygon" %}
| Name                           | Underlying | Strike  | Strike Price | Expiration  | Address                                    |
| ------------------------------ | ---------- | ------- | ------------ | ----------- | ------------------------------------------ |
| PodPut  LINK 27 11 Jun 2021    | LINK       | USDC    | 27           | 11 Jun 2021 | 0x6e20F296E79Cc7a62737fEDCF9A87Fa32F373864 |
| PodPut  WMATIC 1.7 11 Jun 2021 | WMATIC     | USDC    | 1.7          | 11 Jun 2021 | 0xaBD65b1B125e12AbB2F7bDeaE57E62A6272e8797 |
{% endtab %}
{% endtabs %}

## Reserves assets

Reserves refer to the ERC-20 contracts of the underlying assets.

{% hint style="info" %}
We are using the same Kovan testnet assets from Aave, so you can use their faucet:

[https://testnet.aave.com/faucet](https://testnet.aave.com/faucet)\
Or if you want to save some steps, you can go directly on our faucet:

This is to ensure composability in case you want to build bridges like Flashloans with options.
{% endhint %}

{% tabs %}
{% tab title="Mainnet" %}
| Symbol | Address                                                                                                             |
| ------ | ------------------------------------------------------------------------------------------------------------------- |
| USDC   | [0xff970a61a04b1ca14834a43f5de4533ebddb5cc8](https://etherscan.io/token/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48) |
| DAI    | [0x6b175474e89094c44da98b954eedeac495271d0f](https://etherscan.io/token/0x6b175474e89094c44da98b954eedeac495271d0f) |
| aUSDC  |                                                                                                                     |
| WETH   |                                                                                                                     |
| WBTC   |                                                                                                                     |
{% endtab %}

{% tab title="Polygon" %}
| Symbol | Address                                    |
| ------ | ------------------------------------------ |
| DAI    | 0x8f3Cf7ad23Cd3CaDbD9735AFf958023239c6A063 |
| USDC   | 0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174 |
| aUSDC  | 0x9719d867A500Ef117cC201206B8ab51e794d3F82 |
| aDAI   | 0xE0b22E0037B130A9F56bBb537684E6fA18192341 |
| WETH   | 0x7ceB23fD6bC0adD59E62ac25578270cFf1b9f619 |
| WBTC   | 0x1BFD67037B42Cf73acF2047067bd4F2C47D9BfD6 |
| WMATIC | 0x0d500B1d8E8eF31E21C99d1Db9A6444d3ADf1270 |
| LINK   | 0x53e0bca35ec356bd5dddfebbd1fc0fd03fabad39 |
{% endtab %}

{% tab title="Arbitrum" %}


| Symbol | Address                                                                                                            |
| ------ | ------------------------------------------------------------------------------------------------------------------ |
| USDC   | [0xff970a61a04b1ca14834a43f5de4533ebddb5cc8](https://arbiscan.io/token/0xff970a61a04b1ca14834a43f5de4533ebddb5cc8) |
| WETH   | [0x82af49447d8a07e3bd95bd0d56f35241523fbab1](https://arbiscan.io/token/0x82af49447d8a07e3bd95bd0d56f35241523fbab1) |
| WBTC   |                                                                                                                    |
{% endtab %}

{% tab title="Kovan" %}
| Symbol | Address                                                                                                                     |
| ------ | --------------------------------------------------------------------------------------------------------------------------- |
| DAI    | [0xFf795577d9AC8bD7D90Ee22b6C1703490b6512FD](https://kovan.etherscan.io/address/0xFf795577d9AC8bD7D90Ee22b6C1703490b6512FD) |
| USDC   | 0xe22da380ee6b445bb8273c81944adeb6e8450422                                                                                  |
| aUSDC  | [0x02f626c6ccb6d2ebc071c068dc1f02bf5693416a](https://kovan.etherscan.io/address/0x02f626c6ccb6d2ebc071c068dc1f02bf5693416a) |
| aDAI   | 0x58ad4cb396411b691a9aab6f74545b2c5217fe6a                                                                                  |
| WETH   | 0xD7FDf2747A855AC20b96A5cEDeA84b2138cEd280                                                                                  |
| WBTC   | 0x0094e8cf72acf138578e399768879cedd1ddd33c                                                                                  |


{% endtab %}
{% endtabs %}
