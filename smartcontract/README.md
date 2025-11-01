# BaseSafe Factory - Decentralized Savings Pool System

A comprehensive smart contract system for creating and managing decentralized savings pools with multiple pool types to suit different financial needs.

## Overview

BaseSafe Factory is a decentralized finance (DeFi) protocol that enables users to create and participate in various types of savings pools. The system supports three distinct pool types, each designed for different savings strategies and group dynamics.

### Key Features

- **Multiple Pool Types**: Rotational, Target-based, and Flexible savings pools
- **ERC20 Token Support**: Works with any ERC20 token
- **Fee Management**: Configurable treasury and relayer fees
- **Automated Payouts**: Smart contract-managed distributions
- **Member Management**: Secure member verification and access control
- **Penalty System**: Built-in mechanisms to encourage participation

## Pool Types

### 1. Rotational Pool (`BaseSafeRotational`)

A traditional rotating savings and credit association (ROSCA) where members take turns receiving the total pool amount.

**How it works:**
- Fixed number of members contribute a set amount each round
- One member receives the entire pool each round
- Continues until all members have received a payout
- Penalties for non-participation

**Key Parameters:**
- `depositAmount`: Fixed amount each member must contribute per round
- `roundDuration`: Time between payouts
- `treasuryFeeBps`: Fee percentage for treasury (in basis points)
- `relayerFeeBps`: Fee percentage for transaction relayer

**Use Cases:**
- Group savings for large purchases
- Regular investment circles
- Community funding initiatives

### 2. Target Pool (`BaseSafeTarget`)

A goal-oriented savings pool where members collectively work towards a specific target amount.

**How it works:**
- Members contribute varying amounts towards a common goal
- Pool completes when target amount is reached or deadline passes
- Proportional distribution based on contributions
- Treasury fees deducted from total

**Key Parameters:**
- `targetAmount`: Goal amount for the pool
- `deadline`: Maximum time to reach the target
- `treasuryFeeBps`: Fee percentage for treasury

**Use Cases:**
- Crowdfunding projects
- Collective investment goals
- Group purchases with flexible contributions

### 3. Flexible Pool (`BaseSafeFlexible`)

A flexible savings account with optional yield distribution and withdrawal fees.

**How it works:**
- Members can deposit and withdraw at any time
- Minimum deposit requirements
- Optional yield distribution to all members
- Withdrawal fees to discourage frequent withdrawals

**Key Parameters:**
- `minimumDeposit`: Minimum amount for deposits
- `withdrawalFeeBps`: Fee for withdrawals
- `yieldEnabled`: Whether yield distribution is active
- `treasuryFeeBps`: Fee percentage for treasury

**Use Cases:**
- Emergency funds
- Flexible savings with yield
- Community treasury management

## Architecture

### Factory Contract (`BaseSafeFactory`)

The main factory contract that creates and manages all pool instances.

**Functions:**
- `createRotational()`: Deploy a new rotational pool
- `createTarget()`: Deploy a new target pool  
- `createFlexible()`: Deploy a new flexible pool
- `setTreasury()`: Update treasury address (owner only)

### Common Features

All pool types inherit common functionality:
- **Member Management**: Verify and list pool members
- **ERC20 Integration**: Secure token transfers
- **Event Logging**: Comprehensive event emission
- **Access Control**: Owner and member-based permissions

## Getting Started

### Prerequisites

- [Foundry](https://book.getfoundry.sh/getting-started/installation)
- Solidity ^0.8.20
- OpenZeppelin Contracts

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd ajo/smartcontract
```

2. Install dependencies:
```bash
forge install
```

3. Build the contracts:
```bash
forge build
```

4. Run tests:
```bash
forge test
```

### Deployment

1. Set up your environment variables:
```bash
export PRIVATE_KEY=<your-private-key>
export RPC_URL=<your-rpc-url>
export TOKEN_ADDRESS=<erc20-token-address>
export TREASURY_ADDRESS=<treasury-address>
```

2. Deploy the factory:
```bash
forge script script/Deploy.s.sol --rpc-url $RPC_URL --private-key $PRIVATE_KEY --broadcast
```

## Usage Examples

### Creating a Rotational Pool

```solidity
address[] memory members = [0x123..., 0x456..., 0x789...];
uint256 depositAmount = 1000 * 10**18; // 1000 tokens
uint256 roundDuration = 7 days;
uint256 treasuryFee = 100; // 1% (100 basis points)
uint256 relayerFee = 50;   // 0.5% (50 basis points)

address poolAddress = factory.createRotational(
    members,
    depositAmount,
    roundDuration,
    treasuryFee,
    relayerFee
);
```

### Creating a Target Pool

```solidity
address[] memory members = [0x123..., 0x456...];
uint256 targetAmount = 10000 * 10**18; // 10,000 tokens
uint256 deadline = block.timestamp + 30 days;
uint256 treasuryFee = 200; // 2%

address poolAddress = factory.createTarget(
    members,
    targetAmount,
    deadline,
    treasuryFee
);
```

### Creating a Flexible Pool

```solidity
address[] memory members = [0x123..., 0x456...];
uint256 minimumDeposit = 100 * 10**18; // 100 tokens
uint256 withdrawalFee = 50; // 0.5%
bool yieldEnabled = true;
uint256 treasuryFee = 100; // 1%

address poolAddress = factory.createFlexible(
    members,
    minimumDeposit,
    withdrawalFee,
    yieldEnabled,
    treasuryFee
);
```

## Security Considerations

### Access Control
- Only pool members can participate in deposits/contributions
- Owner-only functions for administrative tasks
- Factory owner can update treasury address

### Financial Security
- All token transfers use `transferFrom` with proper error handling
- Penalty system prevents free-riding in rotational pools
- Withdrawal fees discourage excessive withdrawals

### Best Practices
- Always verify member status before transactions
- Use time-based checks for deadlines and payout timing
- Implement proper error handling for all token operations

### Known Limitations
- Members list is fixed at creation (cannot add/remove members)
- No pause functionality for emergency stops
- Penalty collection in rotational pools uses try/catch (may fail silently)

## Testing

Run the test suite:
```bash
forge test -vvv
```

For gas optimization analysis:
```bash
forge snapshot
```

## Gas Optimization

The contracts are optimized for gas efficiency:
- Use of `immutable` variables where possible
- Efficient loops and storage access patterns
- Minimal external calls

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Links

- [Foundry Documentation](https://book.getfoundry.sh/)
- [OpenZeppelin Contracts](https://docs.openzeppelin.com/contracts/)
- [Solidity Documentation](https://docs.soliditylang.org/)

## Disclaimer

This software is provided "as is" without warranty. Use at your own risk. Always audit smart contracts before deploying to mainnet.
