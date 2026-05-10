## ADDED Requirements

### Requirement: Indexed block and transaction API
The system SHALL expose query-oriented APIs for Pearl blocks, transactions, chain tip, mempool state, and confirmations.

#### Scenario: User opens transaction page
- **WHEN** a client requests a transaction by txid
- **THEN** the API returns normalized transaction details, block status, confirmation count, inputs, outputs, fees if derivable, and raw references

### Requirement: Address history API
The system SHALL expose address-level balance and transaction history suitable for wallets, explorers, and payment confirmation flows.

#### Scenario: Wallet requests address history
- **WHEN** a wallet or payment service requests an address summary
- **THEN** the API returns received/spent activity, current balance if indexed, and recent transactions with confirmation state

### Requirement: Miner and useful-work visibility
The system SHALL support app-specific chain-data views/widgets for mining-related blocks, miner addresses, network difficulty/hashrate metrics, and proof/useful-work metadata when available from Pearl sources. It SHALL NOT duplicate Pearl’s public explorer UI unless a product gap is documented.

#### Scenario: User opens miner dashboard
- **WHEN** a user views a miner address or top-miners page
- **THEN** the explorer shows mined blocks/rewards and available network mining metrics without claiming unavailable proof details

### Requirement: Data output decoding
The system SHALL detect and expose OP_RETURN/null-data outputs and other recognized standard Pearl output types for app-specific chain-data and data-anchoring apps.

#### Scenario: Transaction includes OP_RETURN
- **WHEN** an indexed transaction contains a null-data output
- **THEN** the API marks it as data-bearing and exposes safe decoded metadata or raw hex according to configured limits
