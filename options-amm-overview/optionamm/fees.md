---
description: >-
  See below how trading fees are charged and distributed among liquidity
  providers.
---

# Fees

The AMM contract charges a 0.3% fee on every trade that happens in the pool. Since the pool is single-sided, the division of fees happens evenly among the two pools: 50% of the fees go to liquidity providers of token A liquidity, and 50% of the fees go to liquidity providers of token B.

* Fees are always calculated as a relation to token B amount. 
* The contract keeps track of the fee amount in a feePoolA and feePoolB.
* The fair distribution of fees is tracked by the number of shares of each feePool a liquidity provider has. 

## Charging fees

The contract will always charge fees in token B. That means that fees will be charged slightly differently depending on the trade direction and exact input or output.

**Example 1**  
A user wants to buy 3 options \(will be using`exactOutputA`\).

* The contract will calculate the new option price using BS per unit.
* Then it will calculate the total price given to transaction amount \(30 USDC\).
* Then it will add to the total amount the 0.3% fee \(30\*1.003\)=30.009.
* Will slip the fee in half \(0.009/2\) and send it to feePoolA and feePoolB in equal amounts.

**Example 2**  
A user has 100 USDC and wants to buy all of it in options \(will use `exactInputB`\)

* The contract will calculate the new option price using BS per unit.
* Before calculating the total price based on the transaction amount, the contract will impact the fees in the total amount of token B the user is using \(100\*0.003=0.3\) and use the discounted amount\(100-0.3=99.7\) to calculate the total price. 
* Will slip the fee in half \(0.3/2\) and send it to feePoolA and feePoolB in equal amounts.

  \*\*\*\*

## Issuing feePool Shares

When adding liquidity, a user is also "buying" shares of`feePool`it added liquidity for. The new shares emission formula is, for each token pool:

$$
NewShares(A)=\frac{A_i}{Fv_i}
$$

$$
NewShares(B)=\frac{B_i}{Fv_i}
$$

