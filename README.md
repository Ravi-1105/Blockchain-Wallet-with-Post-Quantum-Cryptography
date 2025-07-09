# 🔐 PostQuantumWallet – A Simulated Post-Quantum Secure Ethereum Wallet

This smart contract simulates a **post-quantum cryptographic wallet system** using **keccak256** hashes as stand-ins for quantum-safe signatures. It supports passwordless registration, ETH deposit/withdrawal, and transaction signature verification logic (simulated).

---

## 🚀 Features

- 📌 **User Registration** with auto-generated `publicKeyHash`
- 🔐 **Simulated Signature Verification** using `keccak256`
- 💰 **ETH Deposit and Withdrawal Support**
- 📤 **Internal Transfers** with signature-based verification
- ⚙️ Designed to demonstrate how post-quantum principles could integrate with Solidity

---

## 📄 Smart Contract Overview

### Contract State & Structure
```Solidity
contract PostQuantumWallet {
    struct User {
        bytes32 publicKeyHash;
        bool registered;
    }

    mapping(address => User) public users;
    mapping(address => uint256) public balances;
```
Purpose:

Defines a User with:

publicKeyHash: Simulates a public key fingerprint (e.g., of a Dilithium key)

registered: Tracks whether the user is enrolled

users: Maps Ethereum addresses to their User info

balances: Holds a simulated ETH-like balance per user
