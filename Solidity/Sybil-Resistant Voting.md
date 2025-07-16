# Sybil-Resistant Voting

## Token-Based Voting (ERC20 / ERC721)

> Only token holders can vote, and their influence is based on token balance or ownership.

### Sybil Protection

* Gaining influence requires **buying more tokens**, which costs money — Sybil attacks become economically expensive.

### Implementation Options

* **One-token-one-vote** (fungible)
* **One-wallet-one-vote if holding a specific NFT**
* **Quadratic voting** with token weighting

```solidity
require(token.balanceOf(msg.sender) > 0, "Must hold token to vote");
```

### Extension

* Use a snapshot mechanism (like in **Compound's `Comp`** or **ERC20Votes** from OpenZeppelin) to prevent flash loan attacks.

---

Soulbound NFTs / Identity-NFTs

> Issue **non-transferable** NFTs to verified users.

### Sybil Protection

* One verified identity = one token = one vote

### Tools

* Use ERC-721 with transfer restrictions or adopt **EIP-4973** (Account-bound NFTs)

---

## Gitcoin Passport or BrightID

> Integrate with **off-chain decentralized identity systems** that verify humans.

* Gitcoin Passport scores identity strength using social & web2 signals
* BrightID does web-of-trust verification

Sybil Protection:

* Verification is gated and hard to fake at scale

### Implementation:

* Integrate Passport scores or BrightID checks into your voting contract (via oracle or signature)

---

## Proof-of-Humanity

* Systems like [Proof of Humanity](https://proofofhumanity.id) register real people via video & social proof.
* Only registered humans can vote.

```solidity
require(ProofOfHumanity.isRegistered(msg.sender), "Not verified human");
```

---

## *Quadratic Voting

> Voters can use multiple tokens, but the **cost grows quadratically**.

### Sybil Protection

* Reduces impact of whales while still charging for spammy votes

But you still need identity gating for true Sybil resistance.

---

## What *Won’t* Work

* Limiting vote to `msg.sender` only (anyone can make infinite wallets)
* Tracking IPs or off-chain metadata (can’t be done on-chain)
* CAPTCHA or email verification — impossible to verify on-chain reliably

## Recommended Stack for Sybil-Resistant Voting

| Feature               | Tools/Tech                          |
| --------------------- | ----------------------------------- |
| Identity gating       | Soulbound NFT / Gitcoin Passport    |
| Token staking         | ERC20Votes or NFT-holding check     |
| Vote weighting        | Quadratic voting or 1-token-1-vote  |
| On-chain enforcement  | OpenZeppelin + Snapshot logic       |
| Optional off-chain UI | Snapshot.org, Tally, or custom DApp |

