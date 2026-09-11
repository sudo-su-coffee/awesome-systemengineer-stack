# Fintech & Digital Wallet Architecture

If you want to transition from a basic app to a Fintech giant like **Paytm, Cred, or PhonePe**, the backend engineering completely changes. You are no longer optimizing just for speed; you are optimizing for **Absolute Data Consistency and Compliance**.

## 1. The Immutable Ledger
In an e-commerce app, if an order fails, you just say "Oops, try again." In Fintech, if ₹10,000 disappears during a crash, you get shut down by the government.
- You never update a "balance" directly (e.g., `UPDATE users SET balance = 500`). 
- Instead, you use an **Event-Sourced Ledger**. Every transaction is an immutable append-only row.
- `Transaction 1: +₹1000 (Deposit)`
- `Transaction 2: -₹200 (Purchase)`
- The Go backend calculates the balance dynamically by summing the rows. If the server crashes mid-transaction, nothing is lost.

## 2. Distributed Locks (Preventing Double-Spends)
If a user with a ₹500 balance tries to buy two ₹500 items at the *exact same millisecond*, a standard database might let both through, giving them a negative balance.
- We use **Redis Distributed Locks (Redlock)**. When user `U1` initiates a transaction, Redis completely locks their wallet ID. Any other simultaneous API call is forced to wait until the first transaction finishes.

## 3. KYC & PCI-DSS Compliance
- You cannot store plain-text credit card numbers or Aadhaar IDs.
- Our Go backend integrates with a **Vault Service** (like HashiCorp Vault). The database only stores a secure token, and only the Vault has the key to decrypt the sensitive user data.
