# STX-Deriva

A derivatives trading smart contract built on the Stacks blockchain, enabling leveraged long and short positions with automated liquidation mechanisms.

## Overview

STX-Deriva is a decentralized derivatives trading platform that allows users to:
- Open leveraged long and short positions
- Deposit and withdraw collateral
- Automatically calculate liquidation prices
- Trade with up to configurable leverage ratios

## Features

### Core Trading Features
- **Leveraged Positions**: Open long and short positions with customizable leverage
- **Collateral Management**: Deposit and withdraw STX as collateral
- **Automated Liquidation**: Built-in liquidation price calculation for risk management
- **Position Tracking**: Comprehensive position management with unique IDs

### Smart Contract Components
- **Balance Management**: Track user STX balances within the contract
- **Position Management**: Store and manage trading positions with full metadata
- **Price Oracle**: Simplified price feed system (admin-controlled for testnet)
- **Risk Management**: 150% minimum collateral ratio enforcement

## Contract Architecture

### Data Structures

#### Balances Map
```clarity
(define-map balances principal { stx-balance: uint })
```
Tracks STX balances for each user within the contract.

#### Positions Map
```clarity
(define-map positions uint {
  owner: principal,
  position-type: uint,
  size: uint,
  entry-price: uint,
  leverage: uint,
  collateral: uint,
  liquidation-price: uint
})
```
Stores detailed information about each trading position.

### Constants

- `MIN-COLLATERAL-RATIO`: 150% minimum collateral requirement
- `TYPE-LONG`: 1 (long position identifier)
- `TYPE-SHORT`: 2 (short position identifier)

### Error Codes

- `ERR-UNAUTHORIZED` (100): Unauthorized access attempt
- `ERR-INVALID-AMOUNT` (101): Invalid amount specified
- `ERR-INSUFFICIENT-BALANCE` (102): Insufficient balance for operation
- `ERR-INVALID-POSITION` (103): Invalid position parameters
- `ERR-INSUFFICIENT-COLLATERAL` (104): Insufficient collateral for position

## Public Functions

### Collateral Management

#### `deposit-collateral`
```clarity
(deposit-collateral (amount uint))
```
Deposits STX tokens as collateral into the contract.

#### `withdraw-collateral`
```clarity
(withdraw-collateral (amount uint))
```
Withdraws available STX collateral from the contract.

### Position Management

#### `open-position`
```clarity
(open-position (position-type uint) (size uint) (leverage uint))
```
Opens a new leveraged position:
- `position-type`: 1 for long, 2 for short
- `size`: Position size in STX
- `leverage`: Leverage multiplier

Returns the unique position ID.

#### `close-position`
```clarity
(close-position (position-id uint))
```
Closes an existing position and realizes profit/loss.

### Administrative Functions

#### `update-price`
```clarity
(update-price (new-price uint))
```
Updates the current STX price (admin only). In production, this would be replaced by a decentralized oracle.

#### `set-contract-owner`
```clarity
(set-contract-owner (new-owner principal))
```
Transfers contract ownership (current owner only).

## Read-Only Functions

### `get-balance`
```clarity
(get-balance (user principal))
```
Returns the STX balance for a given user.

### `get-position`
```clarity
(get-position (position-id uint))
```
Returns detailed information about a specific position.

### `get-current-price`
```clarity
(get-current-price)
```
Returns the current STX price from the oracle.

### `calculate-liquidation-price`
```clarity
(calculate-liquidation-price (entry-price uint) (position-type uint) (leverage uint))
```
Calculates the liquidation price for given position parameters.

## Development Setup

### Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) - Stacks smart contract development tool
- Node.js and npm
- TypeScript

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd STX-Deriva
```

2. Install dependencies:
```bash
npm install
```

### Running Tests

Run the test suite:
```bash
npm test
```

Run tests with coverage and cost analysis:
```bash
npm run test:report
```

Watch for file changes and auto-run tests:
```bash
npm run test:watch
```

### Project Structure

```
STX-Deriva/
├── contracts/
│   └── STX-Deriva.clar          # Main smart contract
├── tests/
│   └── STX-Deriva.test.ts       # Test suite
├── settings/
│   ├── Devnet.toml             # Devnet configuration
│   ├── Testnet.toml            # Testnet configuration
│   └── Mainnet.toml            # Mainnet configuration
├── Clarinet.toml               # Clarinet project configuration
├── package.json                # Node.js dependencies
├── tsconfig.json              # TypeScript configuration
└── vitest.config.js           # Test configuration
```

## Network Configurations

The project includes configurations for:
- **Devnet**: Local development environment
- **Testnet**: Stacks testnet deployment
- **Mainnet**: Production deployment (when ready)

## Risk Considerations

⚠️ **Important Security Notes:**

1. **Oracle Dependency**: The current implementation uses a simplified admin-controlled price oracle. Production deployment should integrate with decentralized oracle solutions.

2. **Liquidation Mechanism**: While liquidation prices are calculated, the contract doesn't include automated liquidation execution. This should be implemented before production use.

3. **Collateral Requirements**: The 150% minimum collateral ratio helps manage risk but may not be sufficient in high-volatility scenarios.

4. **Smart Contract Risks**: This is experimental software. Use with caution and consider thorough auditing before mainnet deployment.

## License

This project is licensed under the ISC License.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add/update tests
5. Submit a pull request

## Roadmap

- [ ] Implement automated liquidation mechanism
- [ ] Integrate with decentralized price oracles
- [ ] Add more sophisticated risk management
- [ ] Implement fee structure
- [ ] Add multi-asset support
- [ ] Comprehensive testing and auditing

## Support

For questions and support, please open an issue in the repository.