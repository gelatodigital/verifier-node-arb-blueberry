# Smart Contracts

As part of the Verifier Node package, Gelato provides 3 smart contracts essential for this service:

## 1. NodeKey NFT

NodeKey NFT is the core smart contract for minting NFTs that grant users the right to run a node. This base version provides essential functionality, but projects can customize it to include features like price tiers and transferability options, tailoring it to their specific needs.

### Specifications:

- **ERC-721**
- **Non-Transferable**: NodeKey NFTs cannot be transferred between addresses.
- **Multiple Ownership**: A single address may possess multiple NodeKey NFTs.

## 2. Referee.sol

Referee.sol is a smart contract that enables node key owners to delegate their attestation rights to attesters, who can then attest to every sequencer batch on behalf of the delegators. After the attestation period, a trusted oracle finalizes the attestations and triggers the reward calculation in NodeRewards.sol for successful node key IDs.

### Specifications:

- **Delegation System**: Utilizes delegate.xyz for delegation processes.
- **Attestation Mechanism**:
  - Attesters can attest on behalf of node key owners who have delegated to them.
  - Attesters can attest to every sequencer batch.
- **Finalizing Attestations**: After the designated attestation period, a trusted oracle finalizes all submitted attestations and invokes NodeRewards.sol with the successful node key IDs to handle the rewards calculation.
- **Reward Token & Calculation**: The reward token and reward rate will be handled by NodeRewards.sol and customizable by node sale customers.

## 3. NodeRewards.sol

NodeRewards.sol is a customizable smart contract that handles the reward mechanism for node operators. It provides flexibility for projects to tailor their reward system and payout logic according to their specific requirements. The contract keeps track of rewards for each node key ID and allows node operators to claim their earned rewards. Projects can easily modify the reward calculation and distribution logic within the updateRewards function to align with their desired incentive structure.

### Specifications:

- **Reward Tracking**: Keeps track of the rewards for each node key ID.
- **Updating Rewards**: Referee.sol will call onAttest and onFinalize on NodeRewards.sol to update the rewards.
- **Claiming Rewards**: NodeRewards.sol can mint/transfer the rewards to the node key owners.
