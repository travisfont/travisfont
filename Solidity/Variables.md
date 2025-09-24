# Variables

- **contractCaller** = ``msg.sender``
  - in a constructor function this should be ``i_contractCaller`` as it is immutable: ``address public immutable i_contractCaller;``
- **contractAddress** = ``address(this)``
