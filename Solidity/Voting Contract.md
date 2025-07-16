# Voting Contract


## Features

- `VOTE_THRESHOLD`: Vote passes when yes votes reach this value.
- `executed`: Ensures proposal is only executed once.
- Voting Deadline: timestamp `deadline` to limit the voting per proposal (customizable).
- `executeProposal()`: Separate function to trigger execution (safer than auto-trigger).
  - Use Timelock or Manual Execution
  - Record that proposal passed
- `ProposalStruct` moved to a `mapping` instead of array (due to mapping inside struct).
- `ProposalCreated` and `VoteCast` events.

## Solidity Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Voting {
    enum VoteOption {Absent, Yes, No}

    uint256 public constant VOTE_THRESHOLD = 10;
    uint256 public constant VOTING_DURATION = 3 days;

    struct Proposal {
        address target;
        bytes data;
        uint256 yesCount;
        uint256 noCount;
        uint256 deadline;
        bool executed;
        mapping(address => VoteOption) voteChoice;
    }

    uint256 public proposalCount;
    mapping(uint256 => Proposal) private proposals;

    event ProposalCreated(uint256 indexed proposalId, address indexed creator, address target);
    event VoteCast(uint256 indexed proposalId, address indexed voter, VoteOption vote);
    event ProposalExecuted(uint256 indexed proposalId, address target);

    function newProposal(address _target, bytes calldata _data) external {
        uint256 proposalId = proposalCount++;
        Proposal storage p = proposals[proposalId];
        p.target = _target;
        p.data = _data;
        p.deadline = block.timestamp + VOTING_DURATION;

        emit ProposalCreated(proposalId, msg.sender, _target);
    }

    function castVote(uint256 _proposalId, bool _voteYes) external {
        require(_proposalId < proposalCount, "Invalid proposal ID");

        Proposal storage p = proposals[_proposalId];
        require(block.timestamp <= p.deadline, "Voting period over");
        require(!p.executed, "Proposal already executed");

        VoteOption prevVote = p.voteChoice[msg.sender];
        VoteOption newVote = _voteYes ? VoteOption.Yes : VoteOption.No;
        require(prevVote != newVote, "Vote already cast");

        // Remove previous vote
        if (prevVote == VoteOption.Yes) p.yesCount--;
        if (prevVote == VoteOption.No) p.noCount--;

        // Add new vote
        if (newVote == VoteOption.Yes) p.yesCount++;
        if (newVote == VoteOption.No) p.noCount++;

        // Record vote
        p.voteChoice[msg.sender] = newVote;

        emit VoteCast(_proposalId, msg.sender, newVote);
    }

    function executeProposal(uint256 _proposalId) external {
        require(_proposalId < proposalCount, "Invalid proposal ID");

        Proposal storage p = proposals[_proposalId];
        require(!p.executed, "Proposal already executed");
        require(block.timestamp > p.deadline, "Voting period not yet over");
        require(p.yesCount >= VOTE_THRESHOLD, "Not enough yes votes");

        p.executed = true;

        (bool success, ) = p.target.call(p.data);
        require(success, "Proposal execution failed");

        emit ProposalExecuted(_proposalId, p.target);
    }

    // View helper functions
    function getVote(uint256 _proposalId, address voter) external view returns (VoteOption) {
        require(_proposalId < proposalCount, "Invalid proposal ID");
        return proposals[_proposalId].voteChoice[voter];
    }

    function getCounts(uint256 _proposalId) external view returns (uint256 yes, uint256 no) {
        require(_proposalId < proposalCount, "Invalid proposal ID");
        Proposal storage p = proposals[_proposalId];
        return (p.yesCount, p.noCount);
    }

    function getProposalDeadline(uint256 _proposalId) external view returns (uint256) {
        require(_proposalId < proposalCount, "Invalid proposal ID");
        return proposals[_proposalId].deadline;
    }

    function isProposalExecuted(uint256 _proposalId) external view returns (bool) {
        require(_proposalId < proposalCount, "Invalid proposal ID");
        return proposals[_proposalId].executed;
    }
}
```
## Implementations

### Storage in Structs: Can't Use Mappings in Arrays
**Problem:**
ProposalStruct has a mapping, but it's stored in an array (ProposalList). Solidity disallows returning structs with mappings from public functions and makes handling them non-intuitive.

**Fix:**
Keep the ProposalStruct in a mapping instead of an array.

### Prevent Double Voting
A voter can cast multiple votes and the contract silently replaces the previous vote. Instead, the contract should reject duplicate votes or allow vote updates with stricter checks.

### Proposal Existence Check
There’s no check for whether _proposalId is valid, which can cause out-of-bounds errors.

### Use Events
Emit events for key actions like proposal creation and voting.

### Use uint256 Instead of uint
Using uint256 is recommended for clarity and to match EVM native word size.

## Additional Optional Improvements
- Proposal Execution: Add logic to execute target.call(data) after a threshold.
- Add voting weight based on ERC20 balance (token-based voting)
- Add logging of call return values
- Snapshot Voting: Record voter balances at proposal creation if used in a tokenized voting system.

## Additional Optional Security Enhancements
- - Access Control: Only allow certain roles to create or vote (e.g., only owner or DAO members).
  - Add a whitelist of allowed target contracts/functions
  - Add a proposal registry
  - Only allow admin or multisig to propose
- Limit callable functions via selectors
- Add a timelock delay before execution
- Use delegatecall only if necessary (even more dangerous)
