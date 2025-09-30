```solidity
function getConversionRate(
  uint256               amountETH
  AggregatorV3Interface priceFeed
) internal view returns (uint256)
{
  // ...
}
```

- **Custom Errors** must start with ContractError_XYZ(). Including, be located within a contract unless a requirement disallows it.

```solidity

error ContractError_NotEnoughETH();

function Example() public payable
{
  if (msg.value < fee) {
    revert ContractError_NotEnoughETH();
  }
  
  // this will cost more gas
  // require(msg.value > fee, ContractError_NotEnoughETH());
}

```
- Everytime storage is updated, an event should emitted. **Events must start with EventXYX()**
