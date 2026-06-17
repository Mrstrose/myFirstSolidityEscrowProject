# Escrow Marketplace Smart Contract

A decentralized escrow marketplace built with Solidity that enables secure peer-to-peer transactions between buyers and sellers using blockchain technology.

## Overview

This smart contract creates a trustless marketplace where cryptocurrency funds are held in escrow until both parties fulfill their obligations. An arbitrator can resolve disputes if needed, ensuring protection for all participants.

## How It Works

### User Workflow

1. **Deposit**: Users deposit 1 ETH to participate in the marketplace
2. **Create Listing** (Sellers): Specify an item, price, deadline, and arbitrator
3. **Purchase** (Buyers): Buy items by sending the exact amount—funds are held in escrow
4. **Delivery Confirmation** (Buyers): Approve delivery when satisfied with the item
5. **Withdrawal**: Receive funds after transactions complete

### Transaction States

- **Created**: Listing is active, waiting for a buyer
- **Pending**: Item purchased, awaiting delivery confirmation or dispute
- **Completed**: Buyer approved delivery, seller and buyer can withdraw funds
- **Refunded**: Buyer requested refund after deadline or transaction was canceled
- **Disputed**: Buyer raised a dispute waiting for arbitrator resolution
- **Canceled**: Seller canceled the transaction before purchase
- **Resolved**: Arbitrator resolved the dispute and assigned funds

## Key Features

✅ **Secure Transactions**: Uses `ReentrancyGuard` to prevent reentrancy attacks  
✅ **Dispute Resolution**: Arbitrators can settle disagreements between parties  
✅ **Configurable Fees**: Platform fees are adjustable (capped at 10%)  
✅ **Access Control**: Role-based permissions for sellers, buyers, and arbitrators  
✅ **Automatic Refunds**: Buyers can claim refunds after the deadline expires  
✅ **Fund Tracking**: Separate tracking of deposits and pending withdrawals  

## Contract Parameters

| Parameter | Value |
|-----------|-------|
| Solidity Version | 0.8.30 |
| Required Deposit | 1 ETH |
| Default Fee | 2.5% |
| Maximum Fee Cap | 10% |

## Core Functions

### For Buyers & Sellers

- `deposit()`: Deposit 1 ETH to join the marketplace
- `createTransaction()`: Create a new escrow transaction
- `buyItem(uint256 _id)`: Purchase an item
- `approveDelivery(uint256 _id)`: Confirm item delivery (buyer only)
- `withdrawFunds()`: Withdraw your initial deposit
- `withdraw()`: Withdraw earnings or refunds

### For Sellers

- `cancelEscrow(uint256 _id)`: Cancel a transaction before it's purchased
- `refundBuyer(uint256 _id)`: Issue a refund for a canceled transaction

### For Buyers

- `claimRefund(uint256 _id)`: Claim refund after deadline expires
- `raiseDispute(uint256 _id)`: Raise a dispute during pending transactions

### For Arbitrators

- `resolveDispute(uint256 _id, address _winner)`: Settle disputes by awarding funds to buyer or seller

### For Admin

- `setFee(uint256 newFee)`: Update platform fee
- `setFeeRecipient(address newRecipient)`: Change fee recipient address
- `grantArbitratorRole(address account)`: Add new arbitrators

### Utilities

- `getEscrowDetails(uint256 _id)`: View transaction details (buyer, seller, amount, status)

## Security Features

- **Reentrancy Protection**: All functions that transfer funds use `nonReentrant` modifier
- **Role-Based Access Control**: Uses OpenZeppelin's `AccessControl` for secure permissions
- **Ownership**: Owner-controlled fee management and arbitrator assignments
- **Input Validation**: Comprehensive checks on amounts, addresses, and transaction states

## Installation & Deployment

### Prerequisites

- Solidity compiler 0.8.30 or higher
- OpenZeppelin contracts library

### Dependencies

```
@openzeppelin/contracts/access/Ownable.sol
@openzeppelin/contracts/access/AccessControl.sol
@openzeppelin/contracts/utils/ReentrancyGuard.sol
```

### Deploy

```solidity
// Deploy with initial owner address
EscrowMarketPlace escrow = new EscrowMarketPlace(ownerAddress);
```

## Events

The contract emits the following events for tracking:

- `TransactionCreated`: When a new transaction is created
- `BoughtItem`: When an item is purchased
- `DepositedFunds`: When a user deposits funds
- `DeliveryConfirmed`: When delivery is approved
- `RefundIssued`: When a refund is processed
- `DisputeRaised`: When a dispute is raised
- `DisputeResolved`: When a dispute is resolved
- `EscrowCanceled`: When a transaction is canceled
- `FeeUpdated`: When platform fee is changed
- `FeeRecipientUpdated`: When fee recipient address is changed

## Testing

Recommended testing scenarios:

- [ ] Deposit and withdrawal flow
- [ ] Complete transaction lifecycle (create → buy → deliver → withdraw)
- [ ] Refund after deadline
- [ ] Dispute resolution by arbitrator
- [ ] Fee calculations
- [ ] Reentrancy attack prevention
- [ ] Access control for different roles

## Future Improvements

- Multi-token support (not just ETH)
- Automated dispute resolution with decentralized oracles
- Reputation system for buyers and sellers
- NFT-based arbitrator credentials
- Enhanced dispute evidence submission system

## License

This project is licensed under the MIT License - see the SPDX identifier in the contract.

---

**Note**: This is a learning project. Conduct thorough security audits before deploying to mainnet.
