# Deployed Contracts

Pods Protocol is deployed to the Ethereum Mainnet, as well as the Kovan testnet. The following tables contain the details of the contracts on each of the networks.

If you need development support, join the \#developers channel on our [Pods community Discord server.](https://discord.gg/Qf7utym)

{% tabs %}
{% tab title="Mainnet" %}
| Contracts | ABI | Address |
| :--- | :--- | :--- |
| PodFactory | [JSON]() | 0x |
| WPodFactory | [JSON]() | [0x0]() |
| OptionAMMFactory | [JSON]() | [0x]() |
| BlackScholes |  |  |
| IVProvider |  |  |
| IVGuesser |  |  |
| PriceProvider |  |  |
| NormalDistribution |  |  |
{% endtab %}

{% tab title="Kovan" %}
| Contracts | ABI | Address |
| :--- | :--- | :--- |
| OptionFactory | JSON | 0x46e9E947F9318a9A6a0b2df13A27E2206eae0E3D |
| OptionAMMFactory | JSON | 0x1e4e2b156Bd15Db135c5F6F1f0adda4c185E8A3f |
| OptionExchange | JSON | 0xEEd8e4824465a87d356e35B5ceB7Ab46A8a4B161 |
| BlackScholes | JSON | 0x268238Fc16545298DE942DB6CA4A45D8D7ae5954 |
| IVProvider | JSON | 0x80473756DF77b64EccC0008656e97cd5f64F066d |
| IVGuesser | JSON | 0x80473756DF77b64EccC0008656e97cd5f64F066d |
| NormalDistribution | JSON | 0x9074262d11c69F8001c4F5f773D16cc503db5AE3 |
| PriceProvider | JSON | 0xD913AA8E5b443866c719EA36Eb447ebE96CC0144 |
{% endtab %}
{% endtabs %}

## 

## Option Series

Separate list only with the options that we have deployed during the last year.

{% hint style="info" %}
If you want the address of any option that is not on the list, talk to us on our Discord. 
{% endhint %}

{% tabs %}
{% tab title="Mainnet" %}
| Name | Underlying | Strike  | Strike Price | Expiration | Address |
| :--- | :--- | :--- | :--- | :--- | :--- |
| PodPut  WBTC 12k 31Dec 2020 | WBTC | USDC | 12000 | 31Dec 2020 |  |
| DAI |  |  |  |  | [0x6b175474e89094c44da98b954eedeac495271d0f](https://etherscan.io/address/0x6b175474e89094c44da98b954eedeac495271d0f) |
| USDC |  |  |  |  | [0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48](https://etherscan.io/address/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48) |
| aUSDC |  |  |  |  |  |
| WETH |  |  |  |  |  |
| WBTC |  |  |  |  |  |
{% endtab %}

{% tab title="Kovan" %}

{% endtab %}
{% endtabs %}

## Reserves assets

Reserves refer to the ERC-20 contracts of the underlying assets.

{% hint style="info" %}
We are using the same Kovan testnet assets from Aave, so you can use their faucet:

[https://testnet.aave.com/faucet](https://testnet.aave.com/faucet)  
Or if you want to save some steps, you can go directly on our faucet:

This is to ensure composability in case you want to build bridges like Flashloans with options.
{% endhint %}

{% tabs %}
{% tab title="Mainnet" %}
| Symbol | Address |
| :--- | :--- |
| DAI | [0x6b175474e89094c44da98b954eedeac495271d0f](https://etherscan.io/address/0x6b175474e89094c44da98b954eedeac495271d0f) |
| USDC | [0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48](https://etherscan.io/address/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48) |
| aUSDC |  |
| WETH |  |
| WBTC |  |
{% endtab %}

{% tab title="Kovan" %}
| Symbol | Address |
| :--- | :--- |
| DAI | [0xFf795577d9AC8bD7D90Ee22b6C1703490b6512FD](https://kovan.etherscan.io/address/0xFf795577d9AC8bD7D90Ee22b6C1703490b6512FD) |
| USDC | 0xe22da380ee6b445bb8273c81944adeb6e8450422 |
| aUSDC | [0x02f626c6ccb6d2ebc071c068dc1f02bf5693416a](https://kovan.etherscan.io/address/0x02f626c6ccb6d2ebc071c068dc1f02bf5693416a) |
| aDAI | 0x58ad4cb396411b691a9aab6f74545b2c5217fe6a |
| WETH | 0xD7FDf2747A855AC20b96A5cEDeA84b2138cEd280 |
| WBTC | 0x0094e8cf72acf138578e399768879cedd1ddd33c |
{% endtab %}
{% endtabs %}

