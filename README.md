# SatLink – Bitcoin-Native Micropayment Network

**SatLink** is a high-performance, trustless micropayment infrastructure built on the **Stacks blockchain**, extending Bitcoin with instant, low-fee payment channels. Inspired by Lightning Network concepts, SatLink enables **bidirectional state channels**, **atomic swaps**, and **multi-hop routing** while maintaining Bitcoin-anchored security and user custody.

---

## 🚀 System Overview

SatLink introduces **Layer-2 Bitcoin-native micropayments** using **Stacks smart contracts** for state management.
Key design goals:

* **Trustless**: No custodians; funds always remain under user control.
* **High Throughput**: Near-zero fee transactions at internet speed via off-chain updates.
* **Flexible Settlement**: Cooperative or unilateral closure with dispute resolution.
* **Extensible**: Suitable for IoT, gaming, content monetization, and high-frequency trading.

---

## 📐 Architecture

SatLink leverages **Bitcoin anchoring** via Stacks and organizes functionality into three main layers:

1. **Channel Lifecycle**

   * Create and fund bidirectional payment channels
   * Manage deposits and balances
   * Support cooperative and unilateral closure

2. **Cryptographic Validation**

   * Signature verification for off-chain state updates
   * Nonce-based replay protection
   * Consistent message serialization for signing

3. **Settlement & Security**

   * Cooperative mutual settlement with dual signatures
   * Time-locked unilateral closure with dispute resolution
   * Emergency owner withdrawal (administrative safeguard)

---

## 🔧 Contract Architecture

### Storage

* **`payment-channels`**: Primary mapping keyed by `channel-id` and participant principals.
  Stores balances, deposits, channel state, dispute deadlines, and nonce.

### Constants

* Error codes (`ERR-CHANNEL-NOT-FOUND`, `ERR-INVALID-SIGNATURE`, etc.)
* `CONTRACT-OWNER` for administrative access.

### Core Public Functions

* **`create-channel`** → Open a new payment channel with initial deposit.
* **`fund-channel`** → Increase channel capacity by adding funds.
* **`close-channel-cooperative`** → Mutual closure via dual signatures.
* **`initiate-unilateral-close`** → Start dispute closure with proposed state.
* **`resolve-unilateral-close`** → Settle funds after dispute deadline.
* **`get-channel-info`** → Query channel state.
* **`emergency-withdraw`** → Owner-only safeguard.

### Private Functions

* **Validation helpers** (channel ID, deposits, signatures).
* **Cryptographic utilities** (message construction, serialization, signature verification).

---

## 🔄 Data Flow (Channel Lifecycle)

1. **Channel Creation**

   * Party A calls `create-channel`, deposits STX into escrow.
   * Contract initializes state (balances, nonce, dispute deadline).

2. **Off-Chain Updates**

   * Parties exchange signed state updates reflecting new balances.
   * Nonce ensures replay protection.

3. **Closure Options**

   * **Cooperative:** Both parties submit signatures → funds settled instantly.
   * **Unilateral:** One party submits last signed state → dispute timer starts.
   * After dispute window, funds are released accordingly.

4. **Finalization**

   * Channel is marked closed, balances reset, state cleaned up.

---

## ⚖️ Security Considerations

* **Replay Protection**: Nonce increments with every state update.
* **Signature Validation**: Ensures only valid channel states are accepted.
* **Dispute Resolution**: Time-locked unilateral closure prevents cheating.
* **Escrow Funds**: All channel funds locked in contract until settlement.

---

## 🛠 Development & Testing

SatLink is designed for **Clarinet** testing and simulation.
To run locally:

```bash
clarinet test
clarinet console
```

Contracts follow best practices for **Stacks Clarity** with clear error codes, assertions, and modular validation helpers.

---

## 📌 Use Cases

* **Content Monetization**: Pay-per-view streaming or micropayments for articles.
* **Gaming**: Instant in-game microtransactions between players.
* **IoT**: Machine-to-machine automated payments at sub-cent granularity.
* **High-Frequency Trading**: Off-chain, near-zero latency settlement.

---

## 🛡 License

This project is open-source and available under the **MIT License**.
