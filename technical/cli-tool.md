# Hyvve CLI Tool

The Hyvve CLI (Command Line Interface) tool provides a convenient way to interact with the Hyvve Data Marketplace smart contracts. This document explains how to install, configure, and use the CLI for various operations.

## Installation

### Prerequisites

Before installing the Hyvve CLI, ensure you have:

- Node.js and npm installed
- Movement CLI installed
- An Aptos account with private key

### Installation Steps

1. Clone the repository and install dependencies:

```bash
# Clone the repository
git clone https://github.com/Hyvve-Movement/hyvve-contracts.git

# Navigate to the project directory
cd hyvve-contracts

# Install dependencies
npm install
```

2. Install the CLI globally:

```bash
# Run the installation script
node scripts/setup/install-cli.js
```

3. Set up the Movement CLI:

```bash
movement init --skip-faucet
```

When prompted, choose:

- Network: custom
- Rest endpoint: https://aptos.testnet.bardock.movementlabs.xyz/v1

### Environment Configuration

Create a `.env` file in the project root with the following variables:

```
CAMPAIGN_MANAGER_ADDRESS=<your_address>
PRIVATE_KEY=<your_private_key>

VERIFIER_ADDRESS=
VERIFIER_PRIVATE_KEY=

NETWORK=testnet
RPC_URL=https://aptos.testnet.bardock.movementlabs.xyz/v1
```

**Note**: The VERIFIER_ADDRESS and VERIFIER_PRIVATE_KEY will be generated when registering a new verifier.

### Troubleshooting Installation

If you encounter issues during installation:

1. **Existing installation**:

   ```bash
   rm $(which hyvve-cli)
   node scripts/setup/install-cli.js
   ```

2. **Permission issues**:

   ```bash
   sudo node scripts/setup/install-cli.js
   ```

3. **Alternative usage without global installation**:
   ```bash
   npm run cli -- <category> <command>
   ```

## CLI Categories and Commands

The Hyvve CLI is organized into several categories, each containing specific commands:

### Campaign Management

| Command                 | Description                               | Usage                                                   |
| ----------------------- | ----------------------------------------- | ------------------------------------------------------- |
| `create_campaign`       | Create a new data collection campaign     | `hyvve-cli campaign create_campaign`                    |
| `list_active_campaigns` | List all active campaigns                 | `hyvve-cli campaign list_active_campaigns`              |
| `get_campaign_pubkey`   | Get the public key for a campaign         | `hyvve-cli campaign get_campaign_pubkey <campaign_id>`  |
| `get_remaining_budget`  | Check the remaining budget for a campaign | `hyvve-cli campaign get_remaining_budget <campaign_id>` |

### Contribution Management

| Command               | Description                                  | Usage                                                                                 |
| --------------------- | -------------------------------------------- | ------------------------------------------------------------------------------------- |
| `submit_contribution` | Submit a new data contribution to a campaign | `hyvve-cli contribution submit_contribution <campaign_id> <data_url> <quality_score>` |
| `get_contributions`   | View contributions for a campaign            | `hyvve-cli contribution get_contributions [campaign_id] [contributor_address]`        |

### Campaign Statistics

| Command               | Description                     | Usage                                           |
| --------------------- | ------------------------------- | ----------------------------------------------- |
| `activity`            | View activity statistics        | `hyvve-cli stats activity [address]`            |
| `check_address_stats` | Check statistics for an address | `hyvve-cli stats check_address_stats [address]` |

### Verifier Management

| Command                 | Description             | Usage                                      |
| ----------------------- | ----------------------- | ------------------------------------------ |
| `register_verifier`     | Register a new verifier | `hyvve-cli verifier register_verifier`     |
| `register_verifier_key` | Register a verifier key | `hyvve-cli verifier register_verifier_key` |
| `check_verifier`        | Check verifier status   | `hyvve-cli verifier check_verifier`        |

### Reputation Management

| Command          | Description                   | Usage                                           |
| ---------------- | ----------------------------- | ----------------------------------------------- |
| `get_reputation` | Get reputation for an address | `hyvve-cli reputation get_reputation [address]` |

### Profile Management

| Command               | Description              | Usage                                              |
| --------------------- | ------------------------ | -------------------------------------------------- |
| `manage_profile set`  | Set a new profile        | `hyvve-cli profile manage_profile set <username>`  |
| `manage_profile edit` | Edit an existing profile | `hyvve-cli profile manage_profile edit <username>` |
| `manage_profile view` | View a profile           | `hyvve-cli profile manage_profile view [address]`  |

### Setup and Initialization

| Command               | Description                        | Usage                                 |
| --------------------- | ---------------------------------- | ------------------------------------- |
| `initialize_verifier` | Initialize the verification system | `hyvve-cli setup initialize_verifier` |
| `subscription`        | Set up subscription management     | `hyvve-cli setup subscription`        |

## Usage Examples

### Creating a Campaign

To create a new data collection campaign:

```bash
hyvve-cli campaign create_campaign
```

You will be prompted to enter:

- Campaign title
- Description
- Reward per submission
- Total budget
- Verification criteria
- Start and end dates

### Submitting a Contribution

To submit data to a campaign:

```bash
hyvve-cli contribution submit_contribution <campaign_id> <data_url> <quality_score>
```

For example:

```bash
hyvve-cli contribution submit_contribution 42 https://ipfs.io/ipfs/QmX... 85
```

### Checking Campaign Status

To list all active campaigns:

```bash
hyvve-cli campaign list_active_campaigns
```

### Verifier Operations

To register as a verifier:

```bash
hyvve-cli verifier register_verifier
```

To register a verifier key:

```bash
hyvve-cli verifier register_verifier_key
```

### Getting Help

To see all available categories:

```bash
hyvve-cli --help
```

To see commands in a specific category:

```bash
hyvve-cli campaign --help
```

## Parameter Types

- Parameters in `<angle_brackets>` are required
- Parameters in `[square_brackets]` are optional
- If no address is provided for stats commands, the CLI uses the account from PRIVATE_KEY in .env

## Notes for Specific Commands

### get_contributions

When using `get_contributions`, you have several options:

- Provide `campaign_id` to view all contributions for a campaign
- Provide `contributor_address` to view all contributions by an address
- Provide both to filter contributions by campaign and contributor

For example:

```bash
# Get all contributions for campaign 42
hyvve-cli contribution get_contributions 42

# Get all contributions by a specific address
hyvve-cli contribution get_contributions "" 0x123...

# Get contributions by a specific address for campaign 42
hyvve-cli contribution get_contributions 42 0x123...
```

## Best Practices

When using the Hyvve CLI:

1. **Keep your private keys secure**:

   - Store your private key in the .env file
   - Never commit your .env file to version control
   - Use different keys for development and production

2. **Verify your transactions**:

   - Always check the output of commands to ensure they were successful
   - Use `get_campaign_pubkey` before submitting data to ensure you're using the correct campaign

3. **Regular backups**:

   - Back up your private keys and environment configuration
   - Consider using a secure key management solution

4. **Network selection**:
   - Use testnet for development and testing
   - Only use mainnet for actual production data and campaigns

## Advanced Usage

### Using with npm run

If you prefer not to install the CLI globally, you can use:

```bash
npm run cli -- <category> <command>
```

For example:

```bash
npm run cli -- campaign create_campaign
npm run cli -- contribution submit_contribution 42 https://ipfs.io/ipfs/QmX... 85
```

### Scripting and Automation

For programmatic usage or automation, you can call the CLI from scripts:

```javascript
const { execSync } = require('child_process');

// Example: Create a campaign from a Node.js script
try {
  const result = execSync('hyvve-cli campaign create_campaign', {
    input: JSON.stringify({
      title: 'Automated Campaign',
      description: 'Created via script',
      reward: 1000000,
      budget: 10000000,
      criteria: 'quality>80',
      start: Math.floor(Date.now() / 1000),
      end: Math.floor(Date.now() / 1000) + 86400 * 30, // 30 days
    }),
  });
  console.log(result.toString());
} catch (error) {
  console.error('Error:', error.message);
}
```

## Troubleshooting Common Issues

### Transaction Errors

If you encounter transaction errors:

1. Check your account balance
2. Verify you have the correct permissions
3. Ensure your RPC endpoint is working
4. Check that the campaign or contribution ID exists

### Connection Issues

If you have connection problems:

1. Verify your network connection
2. Check that the RPC_URL in your .env file is correct
3. Ensure the Movement Network is operational

### Command Not Found

If you see "command not found":

1. Verify that the CLI is installed (`which hyvve-cli`)
2. Try reinstalling the CLI
3. Check your PATH environment variable

## Additional Resources

- [Hyvve Contracts Repository](https://github.com/Hyvve-Movement/hyvve-contracts)
- [Smart Contracts Documentation](smart-contracts.md)
- [Blockchain Integration](blockchain-integration.md)
