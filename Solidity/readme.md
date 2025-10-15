# Solidity

## Variables

### Context Variables
- **msg.sender:** The address that called the current function (is address type)
- **msg.value:** The amount of ETH (in wei) sent with a **payable** function call (is uint256 type)
- **msg.data:** The calldata (input data) of the transaction (is bytes type)

### Block Variables
- **block.timestamp:** The current block timestamp (is type uint256)
- **block.number:**  The current block number (is type uint256)

### State Variables (permanently stored on the blockchain in storage)
- By default are the global variables within a contract level were state is not defined.
- For clarity, we label variables as stored in storage using the s_ prefix
- **Visibility:**
    - **public:** readable publically + automatically creates a getter function
    - **private:** not accessible from other contracts
    - **internal:** default visibility + accessible within the current contract and contracts amd child contracts
- **Constant:**
    - global contract level defined
    - only value types (integers, boolean, address, fixed bytes32) and strings can be constants
    - do not take up storage slots (saving gas)
- **Immutable:**
    - assignment can happen in the constructor
    - gas efficient than regular state variables, but less than constants
    - Only value types (integers, boolean, address, fixed bytes32) can be immutable BUT NOT reference types (strings, arrays, mappings, structs, bytes)
- Use **constant** and **immutable** whenever possible to reduces gas costs and create strong data integrity by immutability.

### Storage Variables
> ` uint256[] storage myStorageArray = permanentArray`
- Permanent storage on the blockchain (most expensive)
    - updates the blockchain state 
- Data persists between function calls and transactions (modify state).

### Nemory Variables `memory`
> `uint256[] memory tempArray = new uint256[](inputValues.length)`
- Temporary storage during function execution (cheaper on gas)
- Define as function parameters, return values, and local variables within functions

### Calldata
> `function exampleCall(uint256[] calldata inputValues) external {}`
- Read-only temporary storage for function parameters
= similar to memory variab;es but read-only
- Used for function parameters on **external functions**

## Functions

### Visibility
- **public:** Publically callable function
- **private:** Contract can call the function
- **internal:** Contract and child contractscan call the function
- **external:** Outside contract only callable function

### State Mutability Types
- **view:** Can read but not modify state
- **pure:** Cannot read or modify state
- **payable:** means that the function can be sent ether
- **constructor:** Runs only once when the contract is deployed

## File Organization

```text
project/
├── src/                    # Main contracts
│   ├── MyContract.sol
│   └── utils/
│       └── MathLib.sol
├── script/                 # Deployment scripts (*.s.sol)
│   └── Deploy.s.sol
├── test/                   # Test files (*.t.sol)
│   └── MyContract.t.sol
├── interfaces/             # Interface definitions
│   └── IMyInterface.sol
├── lib/                    # External dependencies
│   └── forge-std/
└── migrations/
    └── 1_deploy_contracts.js
    └── 2_add_features.js
```

## Testing
- [Advanced Deploy Scripts on Foundry](https://updraft.cyfrin.io/courses/foundry/foundry-fund-me/advanced-deploy-scripts)
- [Foundry's Helper Config pattern](https://updraft.cyfrin.io/courses/foundry/foundry-fund-me/refactoring-helper) (enviromental configurations)


### Mock/Test Contracts
- **Mock prefix:** MockToken.sol, MockOracle.sol
- **Test prefix:** TestERC20.sol

### Foundry-specific Conventions

In Solidity, the ***.s.sol** and ***.t.sol** file naming conventions are part of the Foundry development framework's standard structure.

#### *.s.sol - Script Files
> **Deployment and management scripts**

- **Purpose**: Deployment and management scripts
- **Usage**: Used with `forge script` command
- **Examples**:
  - `Deploy.s.sol` - Main deployment script
  - `Upgrade.s.sol` - Contract upgrade scripts
  - `Interact.s.sol` - Interaction scripts

##### Deploy.t.sol
```solidity
import {Script} from "forge-std/Script.sol";
import {Example} from "../src/Example.sol";

contract Deploy is Script
{
    function run() external returns (Example)
    {
        vm.startBroadcast();

        Example contractExample = new Example();
        vm.stopBroadcast();

        return contractExample;
    }
}
```

#### *.t.sol - Test Files
> **Unit and integration tests**

- **Purpose**: Unit tests and integration tests
- **Usage**: Used with `forge test` command
- **Examples**:
  - `MyContract.t.sol` - Tests for MyContract
  - `Integration.t.sol` - Integration tests

##### Example.t.sol
```solidity
import {Test} from "forge-std/Test.sol";
import {MyContract} from "../src/Example.sol";

contract ExampletTest is Test
{
    Example contractExample;
    
    function setUp() public
    {
        contractExample = new Example();
    }
    
    function testExample() public
    {
        assertEq(myContract.getValue(), 42);
    }
}
```
