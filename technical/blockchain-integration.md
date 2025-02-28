# Blockchain Integration

Hyvve leverages the power of blockchain technology to create a secure, transparent, and incentivized data marketplace. This document provides a technical deep dive into how Hyvve integrates with blockchain systems.

## Blockchain Architecture

Hyvve is built on a dual-blockchain architecture:

### Movement Blockchain

The Movement blockchain serves as the primary layer for:

- Smart contract execution
- Token transactions
- Campaign management
- Contributor reputation tracking
- Verification attestations

Movement's EVM compatibility enables us to deploy sophisticated smart contracts while maintaining high throughput and low transaction costs.

### Celestia Blockchain

Celestia functions as our Data Availability (DA) layer:

- Ensures data references and metadata are efficiently stored
- Provides scalability for our decentralized data marketplace
- Enables efficient validation of data availability without requiring nodes to download all data
- Reduces on-chain storage costs while maintaining cryptographic guarantees

## Smart Contract Infrastructure

The Hyvve platform relies on a system of interconnected smart contracts:

### Campaign Contract

```move
module hyvve::campaign {
    use std::string;
    use aptos_framework::coin;
    use aptos_std::table::{Self, Table};
    use aptos_framework::account;
    use aptos_framework::event;

    // Campaign structure definition
    struct Campaign has key, store {
        id: u64,
        creator: address,
        title: string::String,
        description: string::String,
        reward_per_submission: u64,
        total_budget: u64,
        remaining_budget: u64,
        verification_criteria: string::String,
        start_time: u64,
        end_time: u64,
        active: bool,
        submissions: Table<u64, Submission>,
        next_submission_id: u64,
    }

    // Submission structure
    struct Submission has store {
        id: u64,
        contributor: address,
        ipfs_hash: string::String,
        verified: bool,
        verification_time: u64,
        rewarded: bool,
    }

    // Events
    struct CampaignCreatedEvent has drop, store {
        campaign_id: u64,
        creator: address,
        title: string::String,
        total_budget: u64,
    }

    struct SubmissionEvent has drop, store {
        campaign_id: u64,
        submission_id: u64,
        contributor: address,
        ipfs_hash: string::String,
    }

    struct VerificationEvent has drop, store {
        campaign_id: u64,
        submission_id: u64,
        verified: bool,
    }

    // Function to create a new campaign
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

    // Function to submit data to a campaign
    public entry fun submit_data(
        account: &signer,
        campaign_id: u64,
        ipfs_hash: string::String,
    ) {
        // Implementation details...
    }

    // Function for verifiers to validate submissions
    public entry fun verify_submission(
        account: &signer,
        campaign_id: u64,
        submission_id: u64,
        is_valid: bool,
    ) {
        // Implementation details...
    }

    // Function to distribute rewards
    public entry fun distribute_reward(
        account: &signer,
        campaign_id: u64,
        submission_id: u64,
    ) {
        // Implementation details...
    }
}
```

### Reputation Contract

```move
module hyvve::reputation {
    use std::string;
    use aptos_framework::account;
    use aptos_framework::event;

    // Reputation structure
    struct Reputation has key {
        score: u64,
        total_submissions: u64,
        verified_submissions: u64,
        badges: vector<string::String>,
    }

    // Events
    struct ReputationUpdateEvent has drop, store {
        user: address,
        new_score: u64,
        reason: string::String,
    }

    struct BadgeAwardedEvent has drop, store {
        user: address,
        badge: string::String,
    }

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

    // Award badges based on achievements
    public entry fun award_badge(
        admin: &signer,
        user: address,
        badge: string::String,
    ) {
        // Implementation details...
    }
}
```

### Verification Contract

```move
module hyvve::verification {
    use std::string;
    use aptos_framework::account;
    use aptos_framework::event;
    use hyvve::campaign;

    // Verification record structure
    struct VerificationRecord has key, store {
        id: u64,
        campaign_id: u64,
        submission_id: u64,
        ipfs_hash: string::String,
        ai_verification_result: bool,
        verifier: address,
        timestamp: u64,
        verification_data: string::String,
    }

    // Events
    struct VerificationCompletedEvent has drop, store {
        verification_id: u64,
        campaign_id: u64,
        submission_id: u64,
        result: bool,
    }

    // Perform AI verification and record results on-chain
    public entry fun verify_submission(
        verifier: &signer,
        campaign_id: u64,
        submission_id: u64,
        ipfs_hash: string::String,
        verification_result: bool,
        verification_data: string::String,
    ) {
        // Implementation details...
    }

    // Get verification record
    public fun get_verification(
        verification_id: u64
    ): VerificationRecord {
        // Implementation details...
    }
}
```

## On-Chain Verification Process

The verification process in Hyvve combines off-chain AI processing with on-chain attestations:

1. **Data Submission**:

   - Contributor submits data through the frontend
   - Data is encrypted and stored on IPFS
   - IPFS hash and metadata are submitted to the Campaign Contract

2. **AI Verification**:

   - Secure verifier nodes retrieve the data from IPFS
   - AI models analyze the data for authenticity and quality
   - Verification results are prepared for on-chain attestation

3. **On-Chain Attestation**:

   - Verification results are submitted to the Verification Contract
   - A VerificationRecord is created and linked to the submission
   - Events are emitted to notify relevant parties

4. **Reward Distribution**:
   - Based on verification results, the Campaign Contract releases tokens
   - Contributor's reputation is updated through the Reputation Contract
   - Transaction history is permanently recorded on-chain

## Token Economics

Hyvve uses a token-based incentive system:

- **MOVE Tokens**: Used for rewards, governance, and platform fees
- **Staking**: Campaign creators can stake tokens to enhance campaign visibility
- **Reputation Staking**: Contributors can stake tokens to vouch for their data quality

## Cross-Chain Interoperability

While Hyvve primarily operates on Movement and Celestia, we've designed our system with cross-chain interoperability in mind:

1. **Bridge Contracts**: Allow for interaction with other blockchain ecosystems
2. **Token Wrapping**: Enables the use of cross-chain tokens within our ecosystem
3. **Unified Identity**: Contributors can maintain their reputation across different blockchain networks

## Security Considerations

The blockchain integration includes several security measures:

1. **Multi-Signature Authorization**: Critical operations require multiple signatures
2. **Rate Limiting**: Prevents spam attacks on the contract system
3. **Timelock Mechanisms**: Major updates to the protocol include a delay period
4. **Formal Verification**: Our core contracts have undergone formal verification 

## Developer Resources

For developers looking to interact with Hyvve's blockchain components:

- **Contract ABIs**: Available in our [GitHub repository](https://github.com/Hyvve-Movement/hyvve-contracts)
- **SDK**: We provide JavaScript and Python SDKs for easier integration
- **GraphQL API**: Query blockchain data through our specialized API

## Future Enhancements

We are working on several improvements to our blockchain integration:

1. **Layer 2 Scaling**: Implementing zero-knowledge proof systems for improved efficiency
2. **DAO Governance**: Transitioning to a fully decentralized governance model
3. **Cross-Chain Data Marketplaces**: Expanding to additional blockchain ecosystems
4. **Decentralized Verification Nodes**: Distributing the verification process more widely
