---
description: >-
  The fees are made of two parts. The regular 2% fee (base fee) and a dynamic
  fee.
---

# Fees

The AMM contract charges a 2% base fee \(baseFee\) on every trade that happens in the pool. Additionally, an exponential fee \(dynamicFee\) is added to the base fee to deter economic attacks. 

```text
function getCollectable(uint256 amount, uint256 poolAmount) external override view returns (uint256 totalFee) {
        uint256 baseFee = amount.mul(_feeBaseValue).div(10**uint256(_feeDecimals));
        uint256 dynamicFee = _getDynamicFees(amount, poolAmount);
        return baseFee.add(dynamicFee);
    }
```

The calculation for the dynamic fee is dependent on the size of the trade relative to the pool amount A:

$$
Dynamic Fee =\frac{alpha*tradeAmount^3 }{poolAmount^3} / 100
$$

Where parameter $$alpha=2000$$. 

```text
uint256 private immutable _DYNAMIC_FEE_ALPHA = 2000;
```

```text
function _getDynamicFees(uint256 tradeAmount, uint256 poolAmount) internal pure returns (uint256) {
        uint256 numerator = _DYNAMIC_FEE_ALPHA * tradeAmount.mul(tradeAmount).mul(tradeAmount);
        uint256 denominator = poolAmount.mul(poolAmount).mul(poolAmount);
        uint256 ratio = numerator.div(denominator);
        return ratio.mul(tradeAmount) / 100;
    }
```

### Splitting Fees

Since the pool is single-sided, the division of fees happens evenly among the two pools: 50% of the fees go to liquidity providers of token A and 50% of the fees go to liquidity providers of token B.

* Fees are always calculated as a relation to token B amount. 
* The contract keeps track of the fee amount in a feePoolA and feePoolB.
* The fair distribution of fees is tracked by the number of shares of each feePool a liquidity provider has. 

### Charging Fees

The contract will always charge fees in token B. That means that fees will be charged slightly differently depending on the trade direction and exact input or output.

**Example 1**  
A user wants to buy 3 options out of a pool of 30 options \( we will be using`exactOutputA`\).

* The contract will calculate the new option price using BS per unit.
* Then it will calculate the total price given to transaction amount \(50 USDC\).
* Then it will add to the total amount to the base 2% fee and dynamic fee:
  *  $$50*(1+base fee + dynamic fee) =52$$
    * base fee = $$0.02$$ 
    * dynamic fee = $$((2000*3^3)/30^3 ) / 100   = 0.02$$ 
  * Total fees being $$52-50=2$$ 
* Then the total fees get split in half \(2/2\) and sent to feePoolA and feePoolB in equal proportions.

**Example 2**  
A user has 50 USDC and wants to buy all of it in options \(will use `exactInputB`\)

* The contract will calculate the new option price using BS per unit.
* Before calculating the total price based on the transaction amount, the contract will calculate the total fees in the total amount of token B the user is using \(50\*0.04=2\) and use the discounted amount \(50-2=48\) to calculate the total price. 
* The total fee gets cut in half \(2/2\) and sent to feePoolA and feePoolB in equal amounts.

  \*\*\*\*

### Issuing feePool Shares

When adding liquidity, a user is also "buying" shares of`feePool`it added liquidity for. The new shares emission formula is as follows for each token pool:

$$
NewShares(A)=\frac{A_i}{Fv_i}
$$

$$
NewShares(B)=\frac{B_i}{Fv_i}
$$

