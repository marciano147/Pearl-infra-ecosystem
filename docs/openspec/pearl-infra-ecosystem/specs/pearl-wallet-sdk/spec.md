## ADDED Requirements

### Requirement: Address validation primitives
The SDK SHALL validate Pearl addresses by network and type using Pearl-compatible address rules and SHALL expose normalized address metadata.

#### Scenario: User pastes address
- **WHEN** an app validates a pasted Pearl address
- **THEN** the SDK returns whether it is valid, its network, and its address type without needing a running node

### Requirement: Payment request model
The SDK SHALL define a Pearl payment request format covering recipient address, amount, network, memo/reference, expiry, and optional callback metadata.

#### Scenario: Merchant creates invoice
- **WHEN** a merchant creates a Pearl payment request
- **THEN** wallet-compatible clients can parse the request and display the exact destination, amount, and network to the user

### Requirement: Transaction workflow abstraction
The SDK SHALL model the transaction lifecycle as construct, fund, sign, publish, and observe-confirmation steps.

#### Scenario: App sends PRL
- **WHEN** an app initiates a PRL send flow
- **THEN** the SDK can produce or coordinate the necessary wallet/node calls while keeping signing authorization explicit

### Requirement: Future connector compatibility
The SDK SHALL reserve a browser/mobile connector interface for future web wallets without requiring an extension in the first phase.

#### Scenario: Future dapp requests wallet connection
- **WHEN** a connector implementation is added later
- **THEN** it can satisfy the existing connect, getAccounts, signMessage, signTransaction, and sendTransaction interface contracts
