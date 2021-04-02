---
description: >-
  Understand what will be a user's final position when removing liquidity from
  the pool.
---

# Scenarios

{% hint style="info" %}
Please consider a put option ETH:USDC, strike price 320, and expiration one month to all scenarios.
{% endhint %}

## Scenario 1: Providing PodPuts and Removing Liquidity when Option is OTM

Consider the following pool position:

* 50 option tokens
* 10,000 USDC

Of the total balance in the options tokens side, 10 had been provided by Rob in the past.

After trades happened, the current pool position changed to:

* 54 option tokens
* 10,035.65 USDC

Rob decided to remove 100% of the liquidity he provided initially. Option tokens are priced at 0.65 USDC in the moment and it is _out-of-the-money_, considering that the spot price is 325 USDC and the strike price is 320 USDC. The option is live \(not expired\).

According to the AMM's logic, Rob will be removing the combination of:

* 9 option tokens
* 0.67 USDC

This translates into a different position from what was initially provided but keeps Rob's initial exposure.

If Rob wanted to get back to his initial position of 10 options tokens, he could use the 0,67 USDC he received to buy back the "missing" option. Then he can go back to the option instrument contract and unlock his 100% of collateral.

_Position summary:_

| Initial Position | Final Position |
| :--- | :--- |
| 10 option tokens | 9 option tokens |
|  | 0.67 USDC |

## Scenario 2: Providing PodPuts and Removing Liquidity when Option is ITM

Consider the following pool position:

* 50 option tokens
* 10,000 USDC

Of the total balance in the options tokens side, 10 had been provided by Gabriel in the past.

After trades happened, the current pool position changed to:

* 55 option tokens
* 10,023.31 USDC

Gabriel decided to remove 100% of the liquidity he provided initially. Options tokens are priced at 11.03 USDC, and it is _in-the-money_, considering that the spot price is 310 USDC and the strike price is 320 USDC.

According to the AMM's logic, Gabriel will be removing a combination of:

* 9.17 option tokens
* 8.87 USDC 

This translates into a different position from what was initially provided but keeps Gabriel's initial exposure. If Gabriel wanted to get back to his initial position of 10 options tokens, he could use the 8.87 USDC he received to buy back the "missing" portion. Then he can go back to the option instrument contract and unlock his 100% of collateral.

_Position summary:_

| Initial Position | Final Position |
| :--- | :--- |
| 10 option tokens | 9.17 option tokens |
|  | 8.87 USDC |

## Scenario 3: Providing PodPuts and Removing Liquidity when Option is deeply OTM

Consider the following pool position:

* 50 option tokens
* 10,000 USDC

Of the total balance in the options tokens side, 10 had been provided by Babi in the past.

After trades happened, the current pool position changed to:

* 54 option tokens
* 10,002.43 USDC

Babi decided to remove 100% of the liquidity he provided initially. The option tokens are currently priced at 11.03 USDC, and it is _out-of-the-money_, considering that the spot price is 650 USDC and the strike price is 320 USDC.

According to the AMM's logic, Babi will be removing:

* 9 option tokens

This translates into an impermanent loss of 1 option token. But, since the option is _out-of-the-money,_ Babi will be able to withdraw 100% off the collateral she provided initially.

_Position summary:_

| Initial Position | Final Position |
| :--- | :--- |
| 10 option tokens | 9 option tokens |

## Scenario 4: Providing PodPuts and Removing Liquidity when Option is deeply ITM

Consider the following pool position:

* 50 option tokens
* 10,000 USDC

Of the total balance on the stablecoin side, 10 options tokens had been provided by Eri in the past.

After trades happened, the current pool balance when Eri decides to remove 100% of her liquidity is:

* 55 option tokens
* 10,023.31 USDC

Eri decided to remove 100% of the liquidity she provided initially. The option token is currently priced at 170 USDC, and it is _in the money_, considering that the spot price is 150 USDC and the strike price is 320 USDC.

According to the AMM's logic, Eri will be removing a combination of:

* 9.17 option tokens
* 72.09 USDC

This translates into a different position from what was initially provided but keeps Eri's initial exposure. If If Eri wanted to get back to his initial position of 10 options tokens, she could use the 72.09 USDC she received to buy back the "missing" portion. Then he can go back to the option instrument contract and unlock his 100% of collateral.

_Position summary:_

| Initial Position | Final Position |
| :--- | :--- |
| 10 option tokens | 9.17 option tokens |
|  | 72.09 USDC |

## Scenario 5: Providing USDC and Removing Liquidity when Option is OTM

Consider the following pool position:

* 50 option tokens
* 10,000 USDC

Of the total balance on the stablecoin side, 1,000 USD had been provided by Gui in the past.

After trades happened, the current pool position changed to:

* 58 option tokens
* 10,998.92 USDC

Gui decided to remove 100% of the liquidity he provided initially. Options tokens are currently priced at 0 USDC, and it is _out-of-the-money_, considering that the spot price is 500 USDC and the strike price is 320 USDC.

According to the AMM's logic, Gui will be removing a combination of:

* 0.73 option tokens
* 999.90 USDC

This translates into an impermanent loss of 0.1 USDC.

_Position summary:_

| Initial Position | Final Position |
| :--- | :--- |
| 1,000 USDC | 0.73 option tokens |
|  | 999.90 USDC |

## Scenario 6: Providing USDC and Removing Liquidity when Option is ITM

Let's see what happens to Dan when removing liquidity after providing only 1000 USDC to the following PodPut:USDC pool:

* 50 option tokens
* 10,000 USDC

After trades happened, the current pool balance when Dan decides to remove 100% of his liquidity is:

* 58 option tokens
* 10,958.69 USDC

Also, let's consider that the option is currently priced at 170 USDC, and it is in the money, considering that the spot price is 150 USDC and the strike price is 320 USDC.

According to the AMM's logic, Dan will be removing a combination of:

* 0.42 option tokens
* 996.24 USDC

This translates into a different position from what was initially provided but keeps Dan's initial exposure. If Dan wanted to get back to his initial position of 1,000 USDC, he could sell back to the pool the 0.42 options tokens he received and get the equivalent in USDC back.

_Position summary:_

| Initial Position | Final Position |
| :--- | :--- |
| 1,000 USDC | 0.42 option tokens |
|  | 996.24 USDC |

