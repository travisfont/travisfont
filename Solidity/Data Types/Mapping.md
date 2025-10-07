# Mapping (Storage Structure)

> Solidity mappings are gas-efficient for selective data because they only store explicitly set key-value pairs rather than all possible keys.

## Mapping vs Array Differences 
- **Mapping**: A key-value store (like a hash table/dictionary)
- **Array**: An ordered, index-based collection

### Key Differences

| Aspect | Mapping | Array |
|--------|---------|-------|
| **Keys** | Any type (except mapping) | Only `uint` (0, 1, 2, ...) |
| **Order** | No guaranteed order | Maintains insertion order |
| **Iteration** | Cannot iterate over all elements | Can iterate with `for` loops |
| **Default Values** | Returns default value for non-existent keys | Accessing out-of-bounds may revert |
| **Gas Costs** | Consistent lookup cost | Cost increases with array size |

### Code Examples

```solidity
// Mapping example
mapping(address => uint) public balances;

function setBalance(address _user, uint _amount) public {
    balances[_user] = _amount; // Direct key access
}

// Array example  
address[] public users;
mapping(address => uint) public userIndex;

function addUser(address _user) public
{
    users.push(_user); // Ordered insertion
    userIndex[_user] = users.length - 1;
}
```

### When to Use Each

**Use mappings when:**
- You need fast lookups by arbitrary keys
- You don't need to iterate over all elements
- Order doesn't matter

**Use arrays when:**
- You need to maintain order
- You need to iterate over all elements
- You're working with sequential data

