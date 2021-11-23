# OptionPoolRegistry

Contract responsible for registering the Option <> Pool pair relation. It will be used when retriving an `poolAddress` given an `optionAddress`,&#x20;



### getPool

The function returns the address of the pool with given an `_optionAddress. `

| Input name      | Type    | Required | Description                                 |
| --------------- | ------- | -------- | ------------------------------------------- |
| \_optionAddress | address | -        | The address of the option token (PodOption) |



{% tabs %}
{% tab title="Solidity" %}
```javascript
/// Instantiate optionPoolRegistry
IOptionPoolRegistry optionPoolRegistry = IOptionPoolRegistry("/*optionPoolRegistryAddress*/");

address optionAddress = '0xe3..."

optionPoolRegistry.getPool(optionAddress)

// OptionPoolRegistry.sol
     /**
     * @notice Returns the address of a previously created pool
     *
     * @dev If the pool has not been created it will return address(0)
     *
     * @param _optionAddress The address of option token
     * @return The address of the pool
     */
    function getPool(
        address _optionAddress
    ) external returns (address pool) {}
```
{% endtab %}
{% endtabs %}
