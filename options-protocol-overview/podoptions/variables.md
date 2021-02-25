---
description: Find below the variables used in the options protocol.
---

# Variables

## State Variables

The contract calculates and manages the following variables:

$$StrikeReserves_n$$  
The total amount of strike asset balance in an instant before an action takes place. This factor allows visibility on the updated yield generating tokens that may be used as collateral. This variable is not stored on the option contract but on the ERC20 token assigned to be the strike asset.

$$StrikeReserves_i$$  
The total amount of strike asset balance in an instant after an action takes place. This variable is not stored on the option contract but on the ERC20 token assigned to be the strike asset.

$$UnderlyingReserves_n$$  
The total amount of underlying asset balance in an instant before an action takes place. Underlying assets can also accrue interest. $$n$$ stands for "new".

$$UnderlyingReserves_i$$  
The total underlying asset balance after the action took place in a block. 

$$OwnerShares_i$$  
Weighted user's funds served as collateral in light of the current option contract situation. 

$$OwnerShares_t $$  
This factor updates the current total amount of User Weighted Balance.

$$TotalShares_i$$  
This factor represents the sum of all user's $$shares_i$$. 

$$OwnerMintedOptions$$  
Total of options token on the same option series minted by the same user.

### Auxiliary Variables

$$OptionsAmount$$   
Parameter asked at the beginning of minting an option. Describes how many options the user wants to mint and will be used to calculate the collateral requirement further. 

$$AmountToTransfer$$   
The total amount of funds \(either in strike asset or underlying asset\) that will be transferred to the contract to be locked as collateral to mint the requested option amount.

$$StrikeToSend$$   
The variable that will calculate the amount of strike a user will unmint and remove from an unminting of a put. It means how much strike asset the contract will have to send to the user in the unminting process.

$$UnderlyingToSend$$  
The variable that calculates the amount of underlying asset a user will remove from calling the unminting function on a call option token. Meaning how much underlying asset the contract will have to send to the user in the unminting process.

$$OwnerSharesToReduce_w$$  
This variable calculates how much of the collateral that a user currently holds will be removed from the process of unminting options.

### Glossary Comparison

Find below the variables matching the contract names.

| Documentation / Whitepaper | Code | File |
| :--- | :--- | :--- |
|  $$StrikeReserves_n$$ | `strikeReserves` | `(W)PodPut.sol / (W)PodCall.sol / PodOption.sol` |
| $$UnderlyingReserves_n$$  | `underlyingReserves` | `(W)PodPut.sol / (W)PodCall.sol / PodOption.sol` |
| $$OwnerShares_i$$  | `ownerShares` | `(W)PodPut.sol / (W)PodCall.sol / PodOption.sol` |
| $$TotalShares_i$$ | `totalShares` | `(W)PodPut.sol / (W)PodCall.sol / PodOption.sol` |
| $$OwnerMintedOptions$$  | `userMintedOptions` | `(W)PodPut.sol / (W)PodCall.sol / PodOption.sol` |
| $$OptionsAmount$$  | `optionsAmount` | `(W)PodPut.sol / (W)PodCall.sol / PodOption.sol` |
| $$AmountToTransfer$$  | `amountToTransfer` | `(W)PodPut.sol / (W)PodCall.sol / PodOption.sol` |
| $$StrikeToSend$$   | `strikeToSend` | `(W)PodPut.sol / (W)PodCall.sol / PodOption.sol` |
| $$UnderlyingToSend$$  | `underlyingToSend` | `(W)PodPut.sol / (W)PodCall.sol / PodOption.sol` |
| $$OwnerSharesToReduce_w$$  | `ownerSharesToReduce` | `(W)PodPut.sol / (W)PodCall.sol / PodOption.sol` |



