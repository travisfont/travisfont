# Variables

- **contractCaller** = ``msg.sender``
  - in a constructor function this should be ``i_contractCaller`` as it is immutable: ``address public immutable i_contractCaller;``
- **contractAddress** = ``address(this)``

## Principles

- All **magic numbers** are to be constants.
- Contract variables should start with contract, such as ``Fundme contractFundMe = new FundMe(ADDRESS);``
  - This helps keep text capitalization conventions and readability unified.
- All addresses should not hardcoded and should by default be constants.
