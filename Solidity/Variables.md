# Variables

- **contractCaller** = ``msg.sender``
  - in a constructor function this should be ``i_contractCaller`` as it is immutable: ``address public immutable i_contractCaller;``
- **contractAddress** = ``address(this)``

## Principles

- State Varirables which are variables defined outside of functions in a contract should be defined starting with **stateXYZ** to create clarity and readability.
  - This will exlcude any included contracts (eg `HelperConfig`).
  - Example `stateSubscriptionId` 
- All **magic numbers** are to be constants.
- By default, **all variables in a contract** should be **private immutable**, until they are needed publically and require multiple update states.
- Contract variables should start with contract, such as ``Fundme contractFundMe = new FundMe(ADDRESS);``
  - This helps keep text capitalization conventions and readability unified.
- All addresses should not hardcoded and should by default be constants.

```solidity
import {Test} from "forge-std/Test.sol";
import {HelperConfig} from "script/HelperConfig.s.sol";

contract ExampleText is Test
{
  HelperConfig public contractHelperConfig;

  // visibility should always be defined
  address public addressPlayer = makeAddr("player");
  uint256 public constant MAGIC_NUMBER_FEE = 10 ether;

  // state
  uint256 public stateInterval;
  uint256 public stateSubscriptionId;
  uint32  public stateGasLimit

  function setUp() external {}
}
```
