# Blockchain Integration

Hyvve leverages the power of blockchain technology to create a secure, transparent, and incentivized data marketplace. This document provides a technical deep dive into how Hyvve integrates with blockchain systems.

## Blockchain Architecture

Hyvve is built on the Aptos blockchain with integration to the Movement Network:

### Aptos Blockchain Core

The Aptos blockchain serves as the primary layer for:

- Smart contract execution
- Token transactions
- Campaign management
- Contributor reputation tracking
- Verification attestations

Aptos's Move VM provides a secure and efficient environment for our smart contracts, offering high throughput and low transaction costs.

### Movement Network Integration

Hyvve is integrated with the Movement Network, which provides:

- Enhanced scalability for our data marketplace
- Secure transaction processing
- Compatible infrastructure with the existing Aptos ecosystem
- Movement CLI tools for easy interaction with our contracts

## Project Structure

The Hyvve smart contract codebase is organized as follows:

```
├── sources/                # Move smart contracts
│   ├── campaign_manager.move      # Campaign creation and management
│   ├── campaign_state.move        # Campaign state tracking
│   ├── contribution_manager.move  # Contribution submission and verification
│   ├── escrow_manager.move        # Funds management and reward distribution
│   ├── reputation.move            # Contributor reputation system
│   ├── subscription.move          # Subscription management
│   └── verifier.move              # Data verification logic
├── scripts/                # TypeScript CLI tools
│   ├── cli/                # Command-line interface scripts
│   │   ├── campaign/       # Campaign management commands
│   │   ├── contribution/   # Contribution submission commands
│   │   ├── profile/        # User profile management
│   │   ├── reputation/     # Reputation management
│   │   ├── stats/          # Statistics and reporting
│   │   └── verifier/       # Verification tools
│   ├── config/             # Configuration files
│   ├── setup/              # Setup and initialization scripts
│   └── utils/              # Utility functions
└── Move.toml               # Move package configuration
```

## Smart Contract Infrastructure

The Hyvve platform relies on a system of interconnected smart contracts:

### Campaign Manager

The `campaign_manager.move` module handles campaign creation and management:

```move
module hyvve::campaign_manager {
    use std::string;
    use aptos_framework::coin;
    use aptos_std::table::{Self, Table};
    use aptos_framework::account;
    use aptos_framework::event;

    // Campaign structure and functions for campaign management

    // Functions for campaign creation, management, and fund allocation
    public entry fun create_campaign(
        account: &signer,
        title: string::String,
        description: string::String,
        reward_per_submission: u64,
        total_budget: u64,
        verification_criteria: string::String,
        start_time: u64,
        end_time: u64,
    ) {
        // Implementation details...
    }

    // Additional campaign management functions...
}
```

### Contribution Manager

The `contribution_manager.move` module handles data contributions:

```move
module hyvve::contribution_manager {
    use std::string;
    use aptos_framework::account;
    use aptos_framework::event;
    use hyvve::campaign_manager;

    // Contribution structure and functions for submission management

    // Submit a contribution to a campaign
    public entry fun submit_contribution(
        account: &signer,
        campaign_id: u64,
        data_url: string::String,
    ) {
        // Implementation details...
    }

    // Additional contribution management functions...
}
```

### Escrow Manager

The `escrow_manager.move` module handles secure fund management:

```move
module hyvve::escrow_manager {
    use aptos_framework::coin;
    use aptos_framework::account;
    use hyvve::campaign_manager;

    // Functions for secure escrow of funds and reward distribution

    // Distribute rewards for verified contributions
    public entry fun distribute_reward(
        verifier: &signer,
        campaign_id: u64,
        contribution_id: u64,
    ) {
        // Implementation details...
    }

    // Additional escrow management functions...
}
```

### Reputation System

The `reputation.move` module manages contributor reputation:

```move
module hyvve::reputation {
    use std::string;
    use aptos_framework::account;
    use aptos_framework::event;

    // Reputation structure and functions for tracking contributor quality

    // Initialize reputation for a new user
    public entry fun initialize_reputation(account: &signer) {
        // Implementation details...
    }

    // Update reputation based on verified submissions
    public entry fun update_reputation(
        verifier: &signer,
        user: address,
        is_verified: bool,
        quality_score: u64,
    ) {
        // Implementation details...
    }

    // Additional reputation management functions...
}
```

### Verifier

The `verifier.move` module handles the verification process:

```move
module hyvve::verifier {
    use std::string;
    use aptos_framework::account;
    use aptos_framework::event;
    use hyvve::contribution_manager;

    // Verification structure and functions

    // Register a new verifier
    public entry fun register_verifier(
        admin: &signer,
        verifier_address: address,
    ) {
        // Implementation details...
    }

    // Verify a contribution
    public entry fun verify_contribution(
        verifier: &signer,
        campaign_id: u64,
        contribution_id: u64,
        is_valid: bool,
        quality_score: u64,
    ) {
        // Implementation details...
    }

    // Additional verification functions...
}
```

## CLI Tool for Blockchain Interaction

Hyvve provides a comprehensive CLI tool for interacting with the smart contracts:

### Installation

```bash
# Navigate to the project directory
cd hyvve-contracts

# Install dependencies
npm install

# Install the CLI globally
node scripts/setup/install-cli.js

# Setup Movement CLI
movement init --skip-faucet
```

### Campaign Management Commands

| Command                                    | Description                               |
| ------------------------------------------ | ----------------------------------------- |
| `hyvve-cli campaign create_campaign`       | Create a new data collection campaign     |
| `hyvve-cli campaign list_active_campaigns` | List all active campaigns                 |
| `hyvve-cli campaign get_campaign_pubkey`   | Get the public key for a campaign         |
| `hyvve-cli campaign get_remaining_budget`  | Check the remaining budget for a campaign |

### Contribution Management Commands

| Command                                      | Description                                  |
| -------------------------------------------- | -------------------------------------------- |
| `hyvve-cli contribution submit_contribution` | Submit a new data contribution to a campaign |
| `hyvve-cli contribution get_contributions`   | View contributions for a campaign            |

### Verifier Management Commands

| Command                                    | Description             |
| ------------------------------------------ | ----------------------- |
| `hyvve-cli verifier register_verifier`     | Register a new verifier |
| `hyvve-cli verifier register_verifier_key` | Register a verifier key |
| `hyvve-cli verifier check_verifier`        | Check verifier status   |

## On-Chain Verification Process

The verification process in Hyvve combines off-chain AI processing with on-chain attestations:

1. **Data Submission**:

   - Contributor submits data through the frontend or CLI
   - Data is encrypted and stored securely
   - A reference to the data is submitted to the `contribution_manager` contract

2. **AI Verification**:

   - Verifier nodes retrieve the data
   - AI models analyze the data for authenticity and quality
   - Verification results are prepared for on-chain registration

3. **On-Chain Attestation**:

   - Verification results are submitted to the `verifier` contract
   - A verification record is created and linked to the contribution
   - Events are emitted to notify relevant parties

4. **Reward Distribution**:
   - Based on verification results, the `escrow_manager` releases tokens
   - Contributor's reputation is updated through the `reputation` module
   - Transaction history is permanently recorded on-chain

## Key Features

1. **Secure Escrow System**: Funds are locked in escrow until contributions are verified
2. **Quality-Based Rewards**: Contributors are rewarded based on the quality of their submissions
3. **Reputation System**: Track and reward reliable contributors
4. **Automated Verification**: Built-in verification mechanisms to ensure data quality
5. **Platform Fee Structure**: Configurable fee structure for platform sustainability

## Environment Configuration

The project uses environment variables for configuration:

```
CAMPAIGN_MANAGER_ADDRESS=0x1
PRIVATE_KEY=0x1

VERIFIER_ADDRESS=
VERIFIER_PRIVATE_KEY=

NETWORK=testnet
RPC_URL=https://aptos.testnet.bardock.movementlabs.xyz/v1
```

## Security Considerations

The blockchain integration includes several security measures:

1. **Secure Escrow**: Funds are locked in escrow until contributions are verified
2. **Verifier Authentication**: Only registered verifiers can validate contributions
3. **Rate Limiting**: Prevents spam attacks on the contract system
4. **Access Control**: Clear separation of roles and permissions in all contracts

## Developer Resources

For developers looking to interact with Hyvve's blockchain components:

- **Smart Contracts**: Available in our [GitHub repository](https://github.com/Hyvve-Movement/hyvve-contracts)
- **CLI Tool**: Comprehensive command-line interface for interacting with the platform
- **Documentation**: Detailed documentation for all smart contract modules and functions

## Future Enhancements

We are working on several improvements to our blockchain integration:

1. **Enhanced Scalability**: Optimizing contract performance for high throughput
2. **Advanced Verification Mechanisms**: Implementing more sophisticated verification algorithms
3. **Decentralized Governance**: Introducing governance mechanisms for platform decisions
