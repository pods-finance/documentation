---
description: Find below the functions allowed by the options contracts.
---

# Functions

## Main Functions

Once an option is created, a set of 4 actions can happen within the options contract:

* **mint** Users can lock collateral in the contract and mint \(or create\) option tokens \(PodCalls or PodPuts\) at any time before expiration.
* **unmint** A user that _minted_ options previously can take action to leave the position at any time before expiration. To do so, the user must hold the options tokens amount in their wallet to unlock the unmint function. If the user sold those options previously they can buy them back on the AMM and leave the position. Option tokens are burned during the _unmint_ process. If the option is American and any exercise happened between the moment the mint and the unmint, the seller will receive the collateral that was previously locked in the contract during the minting process. 
* **exercise** A user that bought or received options in a wallet can call the exercise function during the exercise period. The contract will require the user to send options tokens and the strike asset \(in the case of a call\) or the underlying asset \(in the case of a put\) to unlock the collateral. Users can exercise American options at any time until expiration or European options during the exercise window period.
* **withdraw**  
  A user that previously minted options can withdraw the eligible funds after expiration. The withdrawal may be composed of a combination of strike assets and underlying assets. 

 

Other actions such as trade and provide liquidity can happen in the [Options AMM](https://app.gitbook.com/@pods-finance-1/s/teste/~/drafts/-MUJTxilQ4CgSOs_VLiH/options-protocol-overview/podoptions/functions).

{% hint style="success" %}
Now that you know the basics of the functions, let's learn about them in more detail. 
{% endhint %}

