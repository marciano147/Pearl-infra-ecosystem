## ADDED Requirements

### Requirement: Pearl network configuration
The system SHALL define canonical Pearl network configuration for mainnet, testnet, and local development, including RPC ports, P2P ports, wallet server ports, address prefixes, and known peers where available.

#### Scenario: App selects network
- **WHEN** a service or SDK is initialized for a Pearl network
- **THEN** it uses the canonical configuration for ports, prefixes, and RPC endpoints without hardcoded scattered constants

### Requirement: Node RPC adapter
The system SHALL provide a typed adapter for safe `pearld` JSON-RPC methods used by apps, including block, transaction, mempool, network, difficulty, and raw transaction broadcast calls.

#### Scenario: Explorer requests latest block
- **WHEN** the indexer or explorer requests chain tip data
- **THEN** the adapter retrieves best block and height through `pearld` RPC and normalizes the result for app services

### Requirement: Wallet RPC adapter
The system SHALL provide a typed adapter for `oyster` wallet RPC methods needed by wallets and payments, including address creation, balance, transaction history, transaction funding, signing, publishing, lock/unlock, and validation flows.

#### Scenario: Payment service creates receiving address
- **WHEN** Pearl Pay needs a receiving address for an invoice
- **THEN** the wallet adapter obtains a new Pearl address through `oyster` without exposing private keys or passphrases to application logs
