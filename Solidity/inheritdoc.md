# Solidity @inheritdoc Documentation Tag

`@inheritdoc` is a NatSpec documentation tag in Solidity that allows you to inherit documentation from parent contracts, interfaces, or overridden functions without having to rewrite the same documentation.

## How it works

When you use `@inheritdoc`, the compiler automatically copies the documentation from the function you're overriding or implementing. This helps maintain consistency and reduces duplication in your codebase.

## Basic syntax

```solidity
/**
 * @inheritdoc InterfaceName
 */
```

or

```solidity
/**
 * @inheritdoc ParentContract
 */
```

## Common use cases

**Interface implementation:**
```solidity
interface IERC20 {
    /**
     * @dev Transfers tokens to a specified address
     * @param to The address to transfer to
     * @param amount The amount to transfer
     * @return success Whether the transfer was successful
     */
    function transfer(address to, uint256 amount) external returns (bool);
}

contract MyToken is IERC20 {
    /**
     * @inheritdoc IERC20
     */
    function transfer(address to, uint256 amount) external returns (bool) {
        // implementation
    }
}
```

**Contract inheritance:**
```solidity
contract BaseContract {
    /**
     * @dev Updates the contract state
     * @param newValue The new value to set
     */
    function updateState(uint256 newValue) public virtual {
        // base implementation
    }
}

contract DerivedContract is BaseContract {
    /**
     * @inheritdoc BaseContract
     */
    function updateState(uint256 newValue) public override {
        // derived implementation
    }
}
```

## Benefits

The `@inheritdoc` tag promotes better documentation practices by ensuring that function documentation stays consistent across inheritance hierarchies while reducing the maintenance burden of keeping duplicate documentation in sync. It's particularly useful in complex contract systems where the same functions are implemented across multiple contracts.
