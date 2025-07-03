# Digital Asset Gaming Ecosystem (DAGE) Smart Contract 

## Overview

The Digital Asset Gaming Ecosystem (DAGE) is an advanced blockchain gaming platform built on the Stacks blockchain that empowers developers and players to create, trade, upgrade, and manage digital gaming assets as NFTs. The platform features comprehensive marketplace functionality, crafting systems, and complete asset lifecycle management.

## Features

### Core Gaming Features
- **Digital Asset Creation**: Create unique gaming assets with rich metadata
- **Asset Crafting System**: Combine assets to create new ones using blueprints
- **Rarity Classification**: 10-tier rarity system for asset valuation
- **Asset Upgrading**: Transform existing assets through crafting recipes

### Marketplace
- **Decentralized Trading**: Buy and sell assets directly on-chain
- **Flexible Pricing**: Set custom prices and expiration dates
- **Commission System**: Configurable platform fees (default 2.5%)
- **Batch Operations**: Execute multiple transfers in single transactions

### Access Management
- **Creator Authorization**: Controlled asset creation permissions
- **Platform Administration**: Comprehensive admin controls
- **Ownership Verification**: Secure asset ownership tracking

### Technical Features
- **SIP-009 Compliant**: Fully implements Stacks NFT standard
- **Comprehensive Validation**: Robust input validation and error handling
- **Emergency Controls**: Admin functions for platform maintenance
- **Analytics Ready**: Built-in statistics and reporting functions

## Smart Contract Architecture

### Core Data Structures

#### Digital Asset Registry
Stores complete asset metadata including:
- Display name and description
- Media URI and classification
- Creator information and characteristics
- Rarity level and trading permissions
- Creation timestamp and extended metadata

#### Ownership Ledger
Tracks asset ownership with:
- Asset ID and wallet address mapping
- Quantity-based ownership (supports fractional ownership)
- Efficient balance queries

#### Marketplace System
Manages trading offers with:
- Seller information and pricing
- Expiration blocks and availability
- Active offer indexing for efficient queries

#### Crafting System
Enables asset creation through:
- Blueprint definitions with material requirements
- Base asset and additional material specifications
- Result asset configuration

## Usage Examples

### Creating Digital Assets

```javascript
// Grant creator authorization (admin only)
await contractCall({
  contractName: 'dage',
  functionName: 'grant-creator-authorization',
  functionArgs: [standardPrincipalCV('SP1CREATOR...')],
  senderKey: adminPrivateKey
});

// Create a new digital asset
await contractCall({
  contractName: 'dage',
  functionName: 'create-new-digital-asset',
  functionArgs: [
    stringAsciiCV('Epic Sword'),
    stringUtf8CV('A legendary weapon forged in dragon fire'),
    stringUtf8CV('https://example.com/sword.png'),
    stringAsciiCV('weapon'),
    listCV([
      tupleCV({
        'characteristic-type': stringAsciiCV('damage'),
        'characteristic-value': stringUtf8CV('150')
      }),
      tupleCV({
        'characteristic-type': stringAsciiCV('element'),
        'characteristic-value': stringUtf8CV('fire')
      })
    ]),
    someCV(stringUtf8CV('Extended lore about the sword...')),
    uintCV(8), // Rarity level
    trueCV()   // Tradeable
  ],
  senderKey: creatorPrivateKey
});
```

### Minting Assets

```javascript
// Mint assets to a player
await contractCall({
  contractName: 'dage',
  functionName: 'mint-digital-assets',
  functionArgs: [
    uintCV(1), // Asset ID
    uintCV(5), // Quantity
    standardPrincipalCV('SP1PLAYER...')
  ],
  senderKey: creatorPrivateKey
});
```

### Marketplace Operations

```javascript
// Create a marketplace offer
await contractCall({
  contractName: 'dage',
  functionName: 'create-marketplace-trading-offer',
  functionArgs: [
    uintCV(1),     // Asset ID
    uintCV(1000),  // Price per unit (microSTX)
    uintCV(3),     // Quantity available
    uintCV(1000)   // Expiration block
  ],
  senderKey: sellerPrivateKey
});

// Purchase from marketplace
await contractCall({
  contractName: 'dage',
  functionName: 'execute-marketplace-purchase',
  functionArgs: [
    uintCV(1), // Offer ID
    uintCV(2)  // Purchase quantity
  ],
  senderKey: buyerPrivateKey
});
```

### Asset Crafting

```javascript
// Create crafting blueprint (admin only)
await contractCall({
  contractName: 'dage',
  functionName: 'create-asset-crafting-blueprint',
  functionArgs: [
    uintCV(1), // Base asset required
    listCV([
      tupleCV({
        'required-material-id': uintCV(2),
        'required-material-quantity': uintCV(3)
      }),
      tupleCV({
        'required-material-id': uintCV(3),
        'required-material-quantity': uintCV(1)
      })
    ]),
    uintCV(4) // Result asset ID
  ],
  senderKey: adminPrivateKey
});

// Execute crafting
await contractCall({
  contractName: 'dage',
  functionName: 'execute-asset-crafting-process',
  functionArgs: [uintCV(1)], // Blueprint ID
  senderKey: playerPrivateKey
});
```

## API Reference

### Administrative Functions

| Function | Description | Access |
|----------|-------------|---------|
| `transfer-platform-administration` | Transfer admin rights | Admin only |
| `modify-marketplace-commission-rate` | Update platform fees | Admin only |
| `grant-creator-authorization` | Allow asset creation | Admin only |
| `revoke-creator-authorization` | Revoke creation rights | Admin only |

### Asset Management

| Function | Description | Access |
|----------|-------------|---------|
| `create-new-digital-asset` | Create new asset type | Authorized creators |
| `mint-digital-assets` | Mint asset instances | Creator/Admin |
| `execute-digital-asset-transfer` | Transfer assets | Owner/Admin |
| `burn-digital-assets` | Destroy assets | Owner |

### Marketplace Functions

| Function | Description | Access |
|----------|-------------|---------|
| `create-marketplace-trading-offer` | List asset for sale | Asset owner |
| `cancel-marketplace-trading-offer` | Cancel listing | Seller |
| `execute-marketplace-purchase` | Buy from marketplace | Anyone |

### Crafting System

| Function | Description | Access |
|----------|-------------|---------|
| `create-asset-crafting-blueprint` | Define crafting recipe | Admin only |
| `execute-asset-crafting-process` | Craft new asset | Anyone with materials |
| `modify-crafting-blueprint-availability` | Enable/disable recipe | Admin only |

### Query Functions

| Function | Description | Returns |
|----------|-------------|---------|
| `get-digital-asset-complete-details` | Asset metadata | Asset details |
| `get-digital-asset-balance` | User's asset balance | Balance amount |
| `get-marketplace-offer-complete-details` | Offer information | Offer details |
| `get-platform-statistics` | Platform metrics | Statistics object |

## Error Codes

| Code | Error | Description |
|------|-------|-------------|
| 100 | `ERR-ACCESS-DENIED` | Insufficient permissions |
| 101 | `ERR-CREATOR-NOT-AUTHORIZED` | Creator not authorized |
| 102 | `ERR-OWNERSHIP-VERIFICATION-FAILED` | Ownership check failed |
| 103 | `ERR-DIGITAL-ASSET-NOT-FOUND` | Asset doesn't exist |
| 104 | `ERR-ASSET-ALREADY-REGISTERED` | Asset already exists |
| 105 | `ERR-INSUFFICIENT-ASSET-BALANCE` | Not enough assets |
| 106 | `ERR-ASSET-NOT-TRANSFERABLE` | Asset locked for trading |
| 107 | `ERR-TRANSACTION-EXECUTION-FAILED` | Transaction failed |
| 108 | `ERR-PAYMENT-PROCESSING-ERROR` | Payment failed |
| 109 | `ERR-RECIPIENT-SAME-AS-SENDER` | Invalid transfer |
| 110 | `ERR-MARKETPLACE-OFFER-NOT-FOUND` | Offer doesn't exist |
| 111 | `ERR-MARKETPLACE-OFFER-EXPIRED` | Offer expired |
| 112 | `ERR-MARKETPLACE-OFFER-INACTIVE` | Offer not active |
| 113 | `ERR-INVALID-PRICING-CONFIGURATION` | Invalid price |
| 114 | `ERR-INVALID-WALLET-ADDRESS` | Invalid address |
| 115 | `ERR-INVALID-PARAMETER-VALUE` | Invalid parameter |
| 116 | `ERR-EMPTY-TEXT-FIELD` | Empty text field |
| 117 | `ERR-INVALID-ATTRIBUTE-STRUCTURE` | Invalid attributes |
| 118 | `ERR-CRAFTING-RECIPE-NOT-FOUND` | Recipe doesn't exist |

## Security Considerations

### Access Control
- **Multi-level Authorization**: Separate permissions for admin, creators, and users
- **Ownership Verification**: Strict validation of asset ownership before operations
- **Creator Authorization**: Controlled access to asset creation functions

### Input Validation
- **Comprehensive Validation**: All inputs validated for type, length, and format
- **Boundary Checks**: Numeric values checked against defined limits
- **Address Validation**: Principal addresses verified before use

### Economic Security
- **Commission Protection**: Maximum platform fees capped at 10%
- **Balance Verification**: Asset balances checked before transfers
- **Expiration Handling**: Automatic expiration of marketplace offers

### Emergency Controls
- **Trading Suspension**: Ability to pause asset trading in emergencies
- **Blueprint Control**: Admin can disable crafting recipes
- **Platform Maintenance**: Emergency functions for system maintenance