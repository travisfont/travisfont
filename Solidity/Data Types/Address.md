# Address

## Zero Address: address(0)

> In Solidity, **`address(0)`** represents the **zero address**, which is equivalent of a **null address** which is used to represent uninitialized, invalid, or burn addresses in smart contracts.
>
> It is a special Ethereum address where all 20 bytes are set to zero: `0x0000000000000000000000000000000000000000`

## What it Signifies

### 1. **Default/Uninitialized Address**
```solidity
address public owner; // Default value is address(0)

function initialize(address _owner) public {
    require(owner == address(0), "Already initialized");
    owner = _owner;
}
```

### 2. **Burn/Black Hole Address**
```solidity
function burn(uint amount) public {
    _transfer(msg.sender, address(0), amount); // Tokens sent to zero address are effectively burned
}
```

### 3. **Invalid/Null Address Check**
```solidity
function transfer(address to, uint amount) public {
    require(to != address(0), "Invalid recipient"); // Prevent transfers to zero address
    // ... transfer logic
}
```

## Common Use Cases

### Constructor Initialization
```solidity
address public previousOwner;

constructor() {
    previousOwner = address(0); // No previous owner initially
}
```

### ERC-20 Token Operations
```solidity
// In ERC-20 tokens, transferring to address(0) typically burns tokens
function burn(uint256 amount) public {
    _transfer(msg.sender, address(0), amount);
}
```

### Access Control
```solidity
function renounceOwnership() public onlyOwner {
    emit OwnershipTransferred(owner, address(0)); // Ownership is "renounced"
    owner = address(0);
}
```

## Important Behaviors

### **Sending Ether**
```solidity
// Sending ETH to address(0) is possible but generally avoided
address(0).transfer(amount); // Works, but burns the ETH permanently
```

### **Default Return Values**
```solidity
mapping(address => uint) public balances;

// Querying an unset key returns the default value for that type
uint balance = balances[address(0)]; // Returns 0 (default for uint)
```

## Security Considerations

```solidity
// Always validate inputs to prevent accidental use
function setBeneficiary(address _beneficiary) public {
    require(_beneficiary != address(0), "Beneficiary cannot be zero address");
    beneficiary = _beneficiary;
}
```
