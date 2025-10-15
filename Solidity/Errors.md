# Errors

- For naming **custom errors** in solidity use error **ContractName_ErrorNameXYX();** to state what contract the error came from.

## Common Required Structures

- Can be ideal to apply within **modifiers**

```solidity
require(msg.sender == owner, "You are not the owner");
require(block.timestamp >= unlockTime, "Funds are still locked");
require(msg.value > 0, "Must send/deposit some ETH");
require(address(this).balance > 0, "No funds to withdraw");
require(balances[msg.sender] >= amount, "Insufficient balance");
```

- `error InsufficientBalance(address user, uint256 balance, uint256 withdrawAmount)`
  - `revert InsufficientBalance(msg.sender, balances[msg.sender], amount);`
