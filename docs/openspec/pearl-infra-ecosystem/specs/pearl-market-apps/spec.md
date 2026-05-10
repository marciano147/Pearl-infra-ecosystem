## ADDED Requirements

### Requirement: Pearl Pay rails
The system SHALL define service contracts for payment links, invoice status, confirmation tracking, merchant callbacks, and hosted checkout flows using PRL transfers.

#### Scenario: Buyer pays invoice
- **WHEN** PRL is received at the invoice address with required confirmations
- **THEN** the payment service marks the invoice paid and emits the configured merchant callback once

### Requirement: OTC market data rails
The system SHALL ingest and normalize public Pearl OTC market data such as offers, trades, volume, active liquidity, and price levels without requiring custody or trade execution in the first phase.

#### Scenario: User views PRL market
- **WHEN** the market app loads
- **THEN** it shows normalized active buy/sell offers, recent trade stats, and liquidity levels with source timestamps

### Requirement: Escrow flow research boundary
The system SHALL document Pearl-compatible escrow and multisig possibilities separately from production implementation.

#### Scenario: Escrow feature is proposed
- **WHEN** a developer starts an escrow implementation task
- **THEN** the task references verified Pearl script/wallet capabilities and includes a security review gate before mainnet use

### Requirement: AI compute marketplace contracts
The system SHALL define interface contracts for model catalog, GPU operator registration, health checks, inference routing, usage metering, billing, and PRL reward reporting without requiring immediate production marketplace launch.

#### Scenario: GPU operator connects
- **WHEN** an operator registers a Pearl-certified model endpoint
- **THEN** the control plane can track endpoint health, model identity, wallet address, capacity, and usage metrics separately from user billing
