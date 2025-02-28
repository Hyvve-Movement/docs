# Smart Contracts

This document provides detailed information about the smart contracts used in the Hyvve Data Marketplace platform.

## Overview

Hyvve's smart contracts are written in Move, a safe and secure programming language originally developed for the Diem blockchain and now used by Aptos and Movement blockchains. The contracts handle all on-chain operations including campaign management, contribution processing, verification, and reward distribution.

## Contract Modules

### Campaign Manager

The Campaign Manager module is responsible for:

- Creating and configuring data collection campaigns
- Managing campaign lifecycle (active, paused, completed)
- Tracking campaign budgets and reward structures
- Storing campaign metadata and requirements

Key functions include:

- `create_campaign`: Create a new data collection campaign with specified parameters
- `update_campaign`: Modify campaign parameters (if authorized)
- `pause_campaign`: Temporarily pause a campaign
- `resume_campaign`: Resume a paused campaign
- `complete_campaign`: Mark a campaign as completed
- `get_campaign_details`: Retrieve information about a specific campaign

### Campaign State

The Campaign State module manages:

- Campaign status tracking
- Campaign statistics
- Public key management for encrypted data

Key functions include:

- `initialize_campaign_state`: Set up initial state for a new campaign
- `update_campaign_status`: Update the status of a campaign
- `get_campaign_pubkey`: Retrieve the public key used for data encryption
- `get_remaining_budget`: Check the remaining budget for a campaign

### Contribution Manager

The Contribution Manager module handles:

- Submitting contributions to campaigns
- Tracking contribution metadata
- Managing contribution states

Key functions include:

- `submit_contribution`: Submit a new data contribution to a campaign
- `get_contributions`: Retrieve contributions for a campaign or contributor
- `update_contribution_status`: Update the status of a contribution

### Escrow Manager

The Escrow Manager module is responsible for:

- Securely holding campaign funds in escrow
- Distributing rewards to contributors
- Managing platform fees
- Handling refunds for unused campaign budgets

Key functions include:

- `lock_funds`: Lock funds in escrow for a campaign
- `distribute_reward`: Send reward to a contributor for a verified contribution
- `refund_remaining_budget`: Return unused funds to campaign creator
- `collect_platform_fees`: Transfer platform fees to the platform treasury

### Reputation System

The Reputation module manages:

- Contributor reputation scores
- Verification history
- Quality metrics

Key functions include:

- `initialize_reputation`: Set up reputation tracking for a new user
- `update_reputation`: Update a user's reputation based on contribution quality
- `get_reputation`: Retrieve reputation information for a user

### Subscription

The Subscription module handles:

- Premium campaign creator subscriptions
- Recurring payment management
- Subscription tiers and benefits

Key functions include:

- `create_subscription`: Start a new subscription
- `renew_subscription`: Renew an existing subscription
- `cancel_subscription`: Cancel a subscription
- `check_subscription_status`: Verify if a user has an active subscription

### Verifier

The Verifier module manages:

- Registration of authorized verifiers
- Verification of contributions
- Quality assessment of submitted data

Key functions include:

- `register_verifier`: Add a new authorized verifier
- `register_verifier_key`: Register a verification key for secure operations
- `verify_contribution`: Verify and rate a contribution
- `revoke_verifier`: Remove an authorized verifier

## Security Features

The smart contracts implement several security measures:

1. **Role-Based Access Control**: Different functions are restricted to appropriate roles (campaign creators, verifiers, administrators)
2. **Secure Fund Management**: Campaign funds are held in escrow and only released for verified contributions
3. **Input Validation**: All input parameters are validated before processing
4. **Event Emission**: Events are emitted for all significant actions for transparency and auditability

## Interacting with Smart Contracts

There are two main ways to interact with Hyvve's smart contracts:

### 1. Hyvve CLI

The Hyvve CLI provides a user-friendly interface for interacting with the smart contracts. See the [blockchain integration](blockchain-integration.md) document for details on CLI commands.

### 2. Direct Contract Calls

For developers who want to interact directly with the contracts:

```bash
# Example: Create a campaign using the Aptos CLI
aptos move run \
  --function-id $CAMPAIGN_MANAGER_ADDRESS::campaign_manager::create_campaign \
  --args string:"Campaign Title" \
  string:"Campaign Description" \
  u64:1000000 \
  u64:10000000 \
  string:"verification_criteria" \
  u64:$START_TIME \
  u64:$END_TIME
```

## Environment Setup

To interact with the smart contracts, you need to set up your environment:

1. Configure environment variables in a `.env` file:

```
CAMPAIGN_MANAGER_ADDRESS=0x1
PRIVATE_KEY=0x1

VERIFIER_ADDRESS=
VERIFIER_PRIVATE_KEY=

NETWORK=testnet
RPC_URL=https://aptos.testnet.bardock.movementlabs.xyz/v1
```

2. Set up the Movement CLI:

```bash
movement init --skip-faucet
```

3. Register as a verifier (if applicable):

```bash
hyvve-cli verifier register_verifier
hyvve-cli verifier register_verifier_key
```

## Contract Interactions

Here's how key components of the system interact:

```
Campaign Creator ──> Campaign Manager ─┬─> Campaign State
                                       │
                                       └─> Escrow Manager

Contributor ──────> Contribution Manager

Verifier ─────────> Verifier Module ──┬─> Contribution Manager
                                      │
                                      ├─> Escrow Manager
                                      │
                                      └─> Reputation
```

1. Campaign creator creates a campaign through Campaign Manager
2. Campaign Manager initializes Campaign State and locks funds in Escrow Manager
3. Contributor submits data through Contribution Manager
4. Verifier verifies contribution through Verifier Module
5. Verifier Module updates Contribution Manager, triggers Escrow Manager for reward, and updates Reputation

## Technical Specifications

- **Language**: Move
- **Blockchain**: Aptos with Movement Network integration
- **State Management**: On-chain state tables for all critical data
- **Event System**: Comprehensive event emission for all significant actions

## Development and Testing

For developers interested in extending or modifying the smart contracts:

1. Clone the repository:

   ```bash
   git clone https://github.com/Hyvve-Movement/hyvve-contracts.git
   ```

2. Install dependencies:

   ```bash
   cd hyvve-contracts
   npm install
   ```

3. Run tests:

   ```bash
   npm test
   ```

4. Deploy to testnet:
   ```bash
   npm run deploy:testnet
   ```

## Best Practices for Integration

When integrating with Hyvve smart contracts:

1. Always check campaign status before submitting contributions
2. Verify transaction success through event monitoring
3. Implement proper error handling for failed transactions
4. Use the CLI for common operations instead of direct contract calls
5. Keep private keys secure and never hardcode them in applications
