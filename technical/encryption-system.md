# Encryption System

Hyvve's encryption system ensures that all data shared on the platform remains secure and confidential. This document explains the technical implementation of our hybrid encryption approach and how it protects contributor data.

## Hybrid Encryption Architecture

Hyvve employs a hybrid encryption system that combines the strengths of both asymmetric (RSA) and symmetric (AES) encryption:

- **RSA (Asymmetric)**: Used for secure key exchange and authentication
- **AES (Symmetric)**: Used for efficient encryption of the actual data

This hybrid approach provides both the security benefits of public-key cryptography and the performance advantages of symmetric encryption for larger datasets.

## Key Generation and Management

### Campaign Creation Phase

When a campaign creator initiates a new data collection campaign:

1. An RSA key pair (public and private keys) is generated
2. The public key is stored on the blockchain as part of the campaign metadata
3. The private key is provided to the campaign creator for secure offline storage
4. The campaign smart contract retains no access to the private key

```move
// Relevant section from campaign_manager.move
public entry fun create_campaign<CoinType: key>(
    account: &signer,
    campaign_id: String,
    // Other parameters...
    encryption_pub_key: vector<u8>  // RSA public key
) acquires CampaignStore {
    // ...
    let campaign = Campaign {
        // Other fields...
        encryption_pub_key,  // Public key stored on-chain
    };
    // ...
}
```

### Key Access Controls

The RSA public key is made available through a view function for contributors to use during data submission:

```move
#[view]
public fun get_encryption_public_key(
    campaign_store_address: address,
    campaign_id: String
): vector<u8> acquires CampaignStore {
    let campaign_store = borrow_global<CampaignStore>(campaign_store_address);
    let len = vector::length(&campaign_store.campaigns);
    let i = 0;
    while (i < len) {
        let campaign = vector::borrow(&campaign_store.campaigns, i);
        if (campaign.campaign_id == campaign_id) {
            return campaign.encryption_pub_key
        };
        i = i + 1;
    };
    abort error::not_found(ECAMPAIGN_NOT_FOUND)
}
```

## Data Encryption Flow

### 1. Contribution Submission Process

When a contributor submits data to a campaign:

1. The client application retrieves the campaign's public RSA key from the blockchain
2. A random AES-256 key is generated for this specific submission
3. The contributor's data is encrypted using the AES key
4. The AES key itself is encrypted using the campaign's RSA public key
5. Both the encrypted data and the encrypted AES key are stored on IPFS
6. Only a reference to the IPFS location is stored on the blockchain

```
┌─────────────────┐     ┌────────────────┐     ┌─────────────────┐
│ Contributor     │     │ Hyvve Platform │     │ Blockchain      │
│ Data            │────▶│ Client         │────▶│ Smart Contract  │
└─────────────────┘     └────────────────┘     └─────────────────┘
        │                       │                      │
        ▼                       ▼                      ▼
┌─────────────────┐     ┌────────────────┐     ┌─────────────────┐
│ AES Encryption  │     │ RSA Encryption │     │ Campaign Public │
│ of Data         │     │ of AES Key     │     │ Key Storage     │
└─────────────────┘     └────────────────┘     └─────────────────┘
        │                       │
        ▼                       ▼
┌─────────────────────────────────────────────┐
│ IPFS Storage of Encrypted Data + Encrypted  │
│ AES Key                                     │
└─────────────────────────────────────────────┘
```

### 2. Data Decryption Process

When a campaign creator accesses the submitted data:

1. The campaign creator provides their RSA private key
2. The platform retrieves the encrypted data package from IPFS
3. The RSA private key decrypts the AES key
4. The decrypted AES key is used to decrypt the actual data
5. The decrypted data is made available to the campaign creator

```
┌────────────────┐     ┌────────────────┐     ┌─────────────────┐
│ Campaign       │     │ Hyvve Platform │     │ IPFS Storage    │
│ Creator        │────▶│ Client         │────▶│ System          │
└────────────────┘     └────────────────┘     └─────────────────┘
        │                       │                      │
        ▼                       ▼                      ▼
┌────────────────┐     ┌────────────────┐     ┌─────────────────┐
│ RSA Private    │     │ RSA Decryption │     │ Encrypted Data  │
│ Key Provision  │────▶│ of AES Key     │◀────│ + Encrypted     │
└────────────────┘     └────────────────┘     │ AES Key         │
                               │              └─────────────────┘
                               ▼
┌────────────────┐     ┌────────────────┐
│ Decrypted      │◀────│ AES Decryption │
│ Contributor    │     │ of Data        │
│ Data           │     └────────────────┘
└────────────────┘
```

## Security Considerations

### Multi-layered Protection

The hybrid encryption system provides several layers of security:

1. **Data Segregation**: Each contribution uses a unique AES key
2. **Key Isolation**: The private key never exists on the blockchain
3. **Decentralized Storage**: Encrypted data is stored on IPFS, not directly on the blockchain
4. **On-chain References**: Only hash references to encrypted data are stored on-chain

### Protection Against Common Attacks

The encryption system is designed to mitigate:

- **Man-in-the-Middle Attacks**: All sensitive data is encrypted before transmission
- **Blockchain Data Exposure**: Even with full blockchain access, data remains encrypted
- **Brute Force Attempts**: AES-256 encryption provides robust protection against brute force attacks
- **Platform Compromise**: Even if the Hyvve platform is compromised, data remains encrypted without the private keys

## Technical Implementation Details

### Encryption Standards

- **RSA**: 2048-bit key length (minimum) for the asymmetric encryption
- **AES**: 256-bit key length for symmetric encryption, using CBC mode with PKCS#7 padding

### Data Structure

The encrypted data package stored on IPFS follows this structure:

```json
{
  "version": "1.0",
  "metadata": {
    "campaign_id": "string",
    "timestamp": "ISO datetime string",
    "content_type": "string"
  },
  "encryption": {
    "method": "AES-256-CBC",
    "iv": "base64-encoded initialization vector",
    "encrypted_aes_key": "RSA-encrypted AES key (base64-encoded)"
  },
  "data": "AES-encrypted data payload (base64-encoded)"
}
```

### Integration with Smart Contracts

The encryption system is integrated with the campaign smart contracts as seen in the `campaign_manager.move` module:

```move
struct Campaign has store, copy {
    // Other fields...
    encryption_pub_key: vector<u8>, // Public encryption key for RSA
}
```

## Best Practices for Key Management

### For Campaign Creators

1. **Secure Storage**: Store your RSA private key in a secure location, preferably offline
2. **Backup Procedures**: Create secure backups of your private key
3. **No Key Sharing**: Never share your private key with anyone
4. **Key Rotation**: For long-running campaigns, consider setting up a new campaign with fresh keys periodically

### For Contributors

1. **Verify Campaign Authenticity**: Ensure you're submitting to legitimate campaigns
2. **Client-Side Encryption**: When possible, perform encryption on your local device before transmission
3. **Secure Connections**: Always use secure connections (HTTPS) when interacting with the Hyvve platform

## Encryption Workflow Example

### Example: Contributing Image Data

1. A contributor wants to submit an image to a campaign
2. The client application retrieves the campaign's public RSA key from the blockchain
3. The application generates a random AES-256 key
4. The image is encrypted using the AES key
5. The AES key is encrypted using the campaign's RSA public key
6. The encrypted image and encrypted AES key are packaged together and uploaded to IPFS
7. The IPFS hash of the package is submitted to the blockchain as part of the contribution

### Example: Retrieving Contributed Data

1. A campaign creator wants to access submissions
2. They provide their RSA private key to the client application
3. The application retrieves the IPFS hashes of all contributions
4. For each contribution, it downloads the encrypted package from IPFS
5. The RSA private key is used to decrypt the AES key
6. The AES key decrypts the actual data
7. The decrypted data is presented to the campaign creator

## Future Enhancements

The Hyvve team is exploring several enhancements to the encryption system:

1. **Threshold Cryptography**: Allowing multiple authorized parties to decrypt data only when acting together
2. **Zero-Knowledge Proofs**: Enabling verification of data properties without revealing the data itself
3. **Homomorphic Encryption**: Performing computations on encrypted data without decryption
4. **Post-Quantum Cryptography**: Preparing for quantum-resistant encryption algorithms
