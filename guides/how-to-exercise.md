# How To Exercise

In order to exercise an option, is pretty simple. You just need to have the options address and PodOption interface. Do not forget to approve first the underlying 

`PodOption` Interface below:

{% tabs %}
{% tab title="Solidity" %}
```javascript
pragma solidity 0.6.12;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

interface IPodOption is IERC20 {
    enum OptionType { PUT, CALL }
    enum ExerciseType { EUROPEAN, AMERICAN }

    event Mint(address indexed minter, uint256 amount);
    event Unmint(address indexed minter, uint256 amount);
    event Exercise(address indexed exerciser, uint256 amount);
    event Withdraw(address indexed minter, uint256 amount);

    function mint(uint256 amountOfOptions, address owner) external;

    function exercise(uint256 amountOfOptions) external;

    function withdraw() external;

    function unmint(uint256 amountOfOptions) external;

    function optionType() external view returns (OptionType);

    function exerciseType() external view returns (ExerciseType);

    function underlyingAsset() external view returns (address);

    function underlyingAssetDecimals() external view returns (uint8);

    function strikeAsset() external view returns (address);

    function strikeAssetDecimals() external view returns (uint8);

    function strikePrice() external view returns (uint256);

    function strikePriceDecimals() external view returns (uint8);

    function expiration() external view returns (uint256);

    function endOfExerciseWindow() external view returns (uint256);

    function hasExpired() external view returns (bool);

    function isAfterEndOfExerciseWindow() external view returns (bool);

    function strikeToTransfer(uint256 amountOfOptions) external view returns (uint256);

    function getSellerWithdrawAmounts(address owner)
        external
        view
        returns (uint256 strikeAmount, uint256 underlyingAmount);

    function underlyingReserves() external view returns (uint256);

    function strikeReserves() external view returns (uint256);
}

```
{% endtab %}
{% endtabs %}

After allowing the PodOption to transfer the underlying token on your behalf, then you can perform the exercise. Example below:

{% tabs %}
{% tab title="Solidity" %}
```javascript
// 1) Instantiate the Option token using the optionAddress
// You can get a current optionAddress from the deployedContracts 
// section
IPodOption option = IPodOption(optionAddress);

// 2) Approve option contract to spend yours underlyingToken
IERC20 underlyingToken = IERC20(underlyingTokenAddress);

underlyingToken.approve(optionAddress, amountOfUnderlying);

// 3) Call the exercise function giving the amount of options
// to exercise as an input.
option.exercise(amountOfOptions);
```
{% endtab %}
{% endtabs %}

