# Hyvve Documentation

![Hyvve Logo](assets/hyvve-logo.png)

**Unlock Data for AI. Contribute and Earn with Hyvve** 🚀

## Welcome to Hyvve

Welcome to the official documentation for Hyvve, a token-incentivized data marketplace that connects AI researchers, companies, and everyday data contributors. This documentation provides comprehensive guides for both data contributors and campaign creators, as well as in-depth technical documentation.

### About Hyvve

Hyvve is a secure, decentralized platform powered by the Movement and Celestia blockchains, designed to provide a robust, scalable, and efficient way to leverage data for AI model training.

Built for the Mammothon Hackathon, Hyvve fosters the creation of AI-ready datasets and ensures that contributors are fairly compensated for their valuable data. Whether you're an AI developer in need of data or a user willing to contribute, Hyvve provides the infrastructure for seamless data sharing and incentivization.

### Key Features

- **For Contributors:** Text & Image Verification, Reputation System, Token Rewards
- **For Campaign Creators:** Multimodal Data Support, Campaign Management, Bulk Data Export
- **Shared Features:** Hybrid Encryption, Decentralized Storage, Intuitive UI

### Getting Started

This documentation will help you get started with Hyvve, whether you're a contributor looking to earn tokens or a campaign creator seeking quality data for AI training.

- [Quickstart Guide](getting-started/quickstart.md) - Get up and running quickly
- [For Contributors](contributors/overview.md) - Learn how to contribute data and earn tokens
- [For Campaign Creators](campaign-creators/overview.md) - Discover how to create and manage data campaigns

### System Architecture

```
+----------------+              +-------------------+                 +-------------------+
|   Contributor  |  ----> (Data) --->  AI Researcher   |  ----> (Data/Insights) --->  Company      |
+----------------+              +-------------------+                 +-------------------+
          |                             |                                      |
          |                             |                                      |
          |  (Tokens/Rewards)           |  (Tokens/Rewards)                    |
          v                             v                                      v
+-----------------------+   <-->   +------------------------+    <-->   +-------------------+
|  Movement & Celestia  |  (Verifies Data) <-->  Blockchain Network  <-->   |  Secure Verifier    |
|  (Data Availability & |            (Stores & Manages Data)             |   (Tamper-Proof Data) |
|  Smart Contracts)     |                                                      +-------------------+
+-----------------------+
          ^
          |
  (Data Verification)
          |
 +-------------------+
 | AI Verification    |
 |  (OCR, Authenticity)|
 +-------------------+
          |
(On-Chain Attestation)
          |
+----------------------+
| Verified Data Attested|
| on Blockchain         |
+----------------------+
```

### Project Repositories

- **Frontend (Next.js)**: [Hyvve Frontend](https://github.com/Hyvve-Movement/hyvve-frontend)
- **Backend (Python/FastAPI)**: [Hyvve Backend](https://github.com/Hyvve-Movement/hyvve-backend)
- **Smart Contracts (Move Modules)**: [Hyvve Contracts](https://github.com/Hyvve-Movement/hyvve-contracts)

### Need Help?

If you have questions or need assistance, check out our [FAQ](support/faq.md) or [contact our support team](support/contact.md).
