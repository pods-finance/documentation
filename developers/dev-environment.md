# Dev Environment

## Gas Consumption

If you want to know in advance what is the average consumption of the main functions of our system and also what is the cost of deployment in case you want to deploy your own options series or pools, we created that table below using [eth-gas-reporter](https://github.com/cgewecke/eth-gas-reporter) \(and their buidler/harhad plugin\).

| **Function** | Min | Max |  **Avg Gas Cost** |
| :--- | :--- | :--- | :--- |
| **Option Instrument** |  |  |  |
| mint | 97,558 | 183,558 | 160k |
| unmint | 49,965 | 57,737 | 53k |
| exercise | 59,441 | 114,995 | 90k |
| withdraw | 32,502 | 99,888 | 60k |
| createOption | 2,295,441 | 2,609,078 | 2307k |
| **Option AMM** | \*\*\*\* |  |  |
| addLiquidity | 267,755 | 312,695 | 312k |
| removeLiquidity | 262,078 | 277,044 | 270k |
| trade \(exactInput / exactOutput\) | 493,390 | 975,030 | 800k |
| createPool | 4,857,078 | 4,857,126 | 4857k |

{% hint style="info" %}
If you want to know how much USD it will cost, you can easily use one of the transaction calculators like the one from [Ethereum Gas Station](https://ethgasstation.info/calculatorTxV.php).
{% endhint %}

## Tool Set

| Tool | Version |
| :--- | :--- |
| Solc | 0.6.12 |
| Buidler | 1.3.8 |
| Open Zeppelin | 3.0.1 |



