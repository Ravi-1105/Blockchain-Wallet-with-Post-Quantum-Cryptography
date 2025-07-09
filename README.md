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

## Project Structure
```
PostQuantumWallet/
├── contracts/
│   └── PostQuantumWallet.sol
├── README.md
```

## Why Post-Quantum?
The rise of quantum computing poses a threat to classical public-key cryptosystems (like ECDSA). This project explores how:

Wallets could shift to quantum-resistant techniques

Blockchain contracts can support non-ECDSA identities

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

###  Events
```Solidity
    event UserRegistered(address user, bytes32 publicKeyHash);
    event TransactionVerified(address from, address to, uint256 amount);
```
Purpose:

Logs important actions to the blockchain:

UserRegistered: When a new user registers

TransactionVerified: When a “signed” transaction is accepted

### Register User
```solidity

    function registerUser() public {
        require(!users[msg.sender].registered, "User already registered");

        bytes32 fakePublicKeyHash = keccak256(
            abi.encodePacked(msg.sender, block.timestamp)
        );

        users[msg.sender] = User(fakePublicKeyHash, true);
        emit UserRegistered(msg.sender, fakePublicKeyHash);
    }
```
Purpose:

Allows any Ethereum address to register once

Simulates a “post-quantum public key hash” by hashing their address and timestamp

Stores this fake publicKeyHash in the users mapping

### Simulated Transaction & Signature Verification
```solidity

    function sendFunds(address _to, uint256 _amount) public payable {
        require(users[msg.sender].registered, "Sender not registered");
        require(users[_to].registered, "Recipient not registered");
        require(balances[msg.sender] >= _amount, "Insufficient funds");

        bytes32 expectedSignature = generateSignature(msg.sender, _to, _amount);
        bytes32 calculatedHash = keccak256(
            abi.encodePacked(msg.sender, _to, _amount)
        );

        require(calculatedHash == expectedSignature, "Invalid signature");

        balances[msg.sender] -= _amount;
        balances[_to] += _amount;

        emit TransactionVerified(msg.sender, _to, _amount);
    }
```
Purpose:

Transfers tokens (ETH-like value) from sender to receiver

### ETH Withdrawal
```solidity
    function withdraw(uint256 _amount) public {
        require(balances[msg.sender] >= _amount, "Insufficient balance");
        balances[msg.sender] -= _amount;
        payable(msg.sender).transfer(_amount);
    }
```
Purpose:

Allows users to withdraw ETH from their internal balance

Safe: prevents overdraw and sends ETH back to the user's wallet

### Deposit to Another Account
```Solidity
    function depositToAccount(address _user) public payable {
        require(users[_user].registered, "User not registered");
        balances[_user] += msg.value;
    }
}
```
Purpose:

Lets someone deposit ETH into another user’s balance

Useful for third-party top-ups or testing

