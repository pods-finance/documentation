---
description: Variables used for the AMM protocol math formulations.
---

# Variables

## General Variables

$$i$$  
A given instant

$$u$$  
Factors that refers to a user \($$u$$\) balance.

$$du$$  
Time of the event of a user's original deposit.

## State Variables

To perform the events, the contract stores, calculates, and manage the following variables:

$$UB_u$$  
User Balance of a token A or B in a given moment. When adding liquidity for the first time, the $$UB_u$$ will be equal to$$A_{du}$$ .

$$IV_{i-1}$$  
Last period's guessed Implied Volatility by the contract based on the latest trade activity in the AMM.

$$IV_i$$  
Implied Volatility that was guessed by the OptionAMM after the current trade ended and impacted the pool position. This factor will be stored and used only in the next event, which in that case will have become the $$IV_{i-1}$$ . The $$IV_i$$is guessed and should guarantee the condition:

$$\displaystyle f_p(IV_i,MarketData_i)=\frac{-B_i}{A_i}$$

$$TB_i$$  
Pool total Balance of a particular token:

* $$TB_{A_i}$$ Pool's total balance of token A in a given instant $$i$$ .
* $$TB_{B_i}$$ Pool's total balance of token B in a given instant$$i$$ .

$$DB_i$$  
Pool's factor that is not amortized.

* $$DB_{A_i}$$ Pool's deamortized balance of token A in a given instant $$i$$ .
* $$DB_{B_i}$$ Pool's deamortized balance of token B in a given instant$$i$$ .

## Dynamic Variables

Variables controlled by the users.

Variables that will be input by the user in `addLiquidity` or `removeLiquidity`

### User Input

User input for liquidity provision:

* ​ $$A_{du}$$ ​ Total amount of A a user deposited originally.
* ​​ $$B_{du}$$ ​ Total amount of B a user deposited originally.

$$r$$  
Is the proportion of token a user wants to withdraw from each token. 1 would be 100% withdrawal.

* $$r_A$$ is the proportion the user wants to withdraw from the original deposit of token A
* $$r_B$$  is the proportion the user wants to withdraw from the original deposit of token B

### Contract Variables

Find below the variables used by the AMM for calculations.

$$P_i$$  
Token A price in units of Token B calculated by the AMM in a given instant $$i$$ .

$$f_p$$  
The AMM's price function to price the token A \(options\) in units of token B.

$$MarketData_i$$  
The underlying asset's spot price is supplied from an external source of data \(currently by Chainlink\).

$$Fv_i$$  
Pool's opening value factor.

$$mAA$$  
How many token A the user can withdraw for each token A deposited

$$mBB$$  
How many token B the user can withdraw for each token B deposited

$$mBA$$  
How many token A the user can withdraw for each token B deposited

$$mAB$$  
How much token B the user can withdraw for each token A deposited

### Trade formulas

Variables used by the AMM for calculations during a trade:

* $$A_i$$  Trade amount of token A to be transferred or received in a trade event.
* $$B_i$$  Trade amount of token B to be transferred or received in a trade event. 

$$poolAmountA$$  
Is the variable that contains the maximum that could be traded based on the current pool situation and option price for the token A.

$$poolAmountB$$  
Is the variable that contains the maximum that could be traded based on the current pool situation and option price for the token B.

$$k$$  
Product constant.

### Glossary Comparison

Find below the variables matching with the contract names.

| Documentation / Whitepaper | Code | File |
| :--- | :--- | :--- |
| $$UB_A$$ | `UserDepositSnapshot.tokenABalance` | `AMM.sol` |
| $$UB_B$$ | `UserDepositSnapshot.tokenBBalance` | `AMM.sol` |
| $$Fv_{du}$$ | `UserDepositSnapshot.fImp` | `AMM.sol` |
| $$P_i$$ | `ABPrice` | `AMM.sol` |
| $$f_p$$ | `_getABPrice()` | `AMM.sol` |
| $$IV_{i-1}$$ | `PriceProperties.currentSigma` | `OptionAMMPool.sol` |
| $$IV_i$$ | `newIV` | `OptionAMMPool.sol` |
| $$A_i$$ | `exactAmountAIn / exactAmountAOut / amountAIn / amountAOut` | `AMM.sol` |
| \`\`$$B_i$$ | `exactAmountBIn / exactAmountBOut / amountBIn / amountBOut` | `AMM.sol` |
| \`\`$$TB_i$$ | `totalTokenA / totalTokenB` | `AMM.sol` |
| \`\`$$DB_i$$ | `deamortizedTokenABalance / deamortizedTokenBBalance` | `AMM.sol` |
| \`\`$$Fv_i$$ | `fImpOpening` | `AMM.sol` |
| \`\`$$r$$ | `percentA / percentB` | `AMM.sol` |
| \`\`$$mAA$$ | `Mult.AA` | `AMM.sol` |
| $$mBB$$ | `Mult.BB` | `AMM.sol` |
| $$mBA$$ | `Mult.BA` | `AMM.sol` |
| $$mAB$$ | `Mult.AB` | `AMM.sol` |

