# Solidity

## Testing
- [Advanced Deploy Scripts on Foundry](https://updraft.cyfrin.io/courses/foundry/foundry-fund-me/advanced-deploy-scripts)
- [Foundry's Helper Config pattern](https://updraft.cyfrin.io/courses/foundry/foundry-fund-me/refactoring-helper) (enviromental configurations)

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
