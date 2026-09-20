# Master Study Notes: Blockchain Technology & Smart Contracts
## Course Code: 22MCA331 / Professional Elective | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Distributed Ledger Technology & Cryptographic Foundations (Decentralization vs Centralization, The Byzantine Generals Problem, Double Spending, Cryptographic Hashes - SHA-256, Merkle Trees, Digital Signatures via ECC secp256k1).
* **Module 2**: Blockchain Architecture & Data Structure (Block Anatomy: Header, Nonce, Timestamp, Difficulty Target, Merkle Root, Transaction List, Genesis Block, Hard Forks vs Soft Forks).
* **Module 3**: Consensus Algorithms (Proof of Work - PoW, Proof of Stake - PoS, Delegated PoS - DPoS, Practical Byzantine Fault Tolerance - PBFT, 51% Attack & Sybil Attacks).
* **Module 4**: Ethereum & Smart Contract Engineering (Ethereum Virtual Machine - EVM, Solidity Syntax, Storage vs Memory vs Calldata, Gas Mechanics, Reentrancy Attacks & Security Best Practices, ERC-20 & ERC-721 Standards).
* **Module 5**: Enterprise Blockchains & Use Cases (Hyperledger Fabric: Peer Nodes, Ordering Service, Membership Service Provider - MSP, Channels, Decentralized Finance - DeFi, Supply Chain Tracking).

---

# MODULE 1: DISTRIBUTED LEDGER & CRYPTOGRAPHIC PRIMITIVES

## 1.1 The Byzantine Generals Problem & Double Spending
* **The Problem**: Reliable consensus among independent, geographically separated nodes over an untrusted communication channel where some participants may act maliciously or send contradictory information.
* **Double Spending**: The digital vulnerability where a single digital asset or token is spent simultaneously in more than one transaction. Solved in blockchain via chronological timestamping, cryptographic hashing, and distributed consensus.

## 1.2 Merkle Trees (Cryptographic Hash Trees)
```
                          [ Root Hash (H1234) ]
                                /        \
                               /          \
                      [ H12 ]                [ H34 ]
                      /     \                /     \
                   H(Tx1)  H(Tx2)         H(Tx3)  H(Tx4)
                     |       |              |       |
                   [Tx 1]  [Tx 2]         [Tx 3]  [Tx 4]
```
* **Purpose**: Efficient and secure verification of transaction integrity in $O(\log N)$ time.
* **Merkle Proof (Lightweight verification)**: A thin client with only the Block Header (Merkle Root) can verify if transaction $Tx_1$ is included in the block by obtaining only sister hashes $H(Tx_2)$ and $H_{34}$ without downloading the entire blockchain history.

---

# MODULE 2: BLOCKCHAIN ANATOMY & STRUCTURE

## 2.1 Anatomy of a Block
```
+-------------------------------------------------------------+
| BLOCK HEADER                                                |
|  * Previous Block Hash: 000000000019d6689c085ae165831e934...|
|  * Merkle Root Hash:    4a5e1e4baab89f3a32518a88c31bc87f6...|
|  * Timestamp:           1700000000                          |
|  * Difficulty Target:   0x1d00ffff                          |
|  * Nonce:               2083236893                          |
+-------------------------------------------------------------+
| TRANSACTION COUNTER & BODY                                  |
|  * Tx 0 (Coinbase / Mining Reward)                          |
|  * Tx 1 (Alice -> Bob: 1.5 BTC)                             |
|  * Tx 2 (Bob -> Charlie: 0.8 BTC)                           |
+-------------------------------------------------------------+
```

## 2.2 Hard Fork vs. Soft Fork
| Property | Hard Fork | Soft Fork |
| :--- | :--- | :--- |
| **Backward Compatibility**| **Non-backward compatible**: Un-upgraded nodes reject new blocks | **Backward compatible**: Un-upgraded nodes accept new blocks |
| **Rule Modification** | Relaxes or alters validation rules | Tightens or adds more restrictive rules |
| **Blockchain Split** | Permanent split into two chains if not 100% adopted (e.g., BTC & BCH, ETH & ETC) | Single canonical chain preserved if majority mining power upgrades |

---

# MODULE 3: CONSENSUS MECHANISMS

## 3.1 Proof of Work (PoW) vs. Proof of Stake (PoS)
| Feature | Proof of Work (PoW) | Proof of Stake (PoS) |
| :--- | :--- | :--- |
| **Block Proposer Choice** | Solves computational cryptographic puzzle | Proportional to economic stake (validators lock collateral) |
| **Energy Consumption** | Extremely high (Gigawatts consumed by ASICs) | 99.95% lower (Energy-efficient) |
| **Finality Time** | Probabilistic (e.g. 6 Bitcoin block confirmations ~ 60 mins) | Deterministic (e.g., Ethereum Gasper finality ~ 12 mins) |
| **Sybil Attack Resistance**| Cost of physical ASIC compute hardware | Cost of acquiring > 51% of circulating token supply |

---

# MODULE 4: ETHEREUM, EVM & SOLIDITY SMART CONTRACTS

## 4.1 EVM Architecture & Gas System
* **EVM (Ethereum Virtual Machine)**: Turing-complete quasi-Turing stack-based runtime executing bytecode inside every Ethereum node.
* **Gas Concept**:
  $$\text{Transaction Fee} = \text{Gas Units Consumed} \times (\text{Base Fee} + \text{Priority Fee})$$
  * **Gas Limit**: Maximum computational steps the transaction is permitted to take before reverting (prevents infinite loops).
  * **Out of Gas**: If gas runs out, all state modifications revert, but the fee is forfeited to the miner.

## 4.2 Reentrancy Attack & The Checks-Effects-Interactions Pattern
* **Vulnerability**: An external contract calls back into the victim contract before the victim contract updates its state balance.
```solidity
// SECURE PATTERN: Checks-Effects-Interactions
function withdraw(uint256 amount) public {
    // 1. CHECKS
    require(balances[msg.sender] >= amount, "Insufficient funds");

    // 2. EFFECTS (Update state BEFORE sending Ether)
    balances[msg.sender] -= amount;

    // 3. INTERACTIONS (External transfer last)
    (bool success, ) = msg.sender.call{value: amount}("");
    require(success, "Transfer failed");
}
```

---

# MODULE 5: ENTERPRISE BLOCKCHAINS (HYPERLEDGER FABRIC)

## 5.1 Hyperledger Fabric Architecture
* **Permissioned Network**: Participants are verified entities identified via X.509 certificates generated by a **Membership Service Provider (MSP)**.
* **Execute-Order-Validate Architecture**:
  1. **Execution (Endorsement)**: Client sends transaction proposal to Endorsing Peers. Peers simulate the chaincode and return cryptographic endorsements.
  2. **Ordering**: Ordering Service (Raft consensus) packages endorsed transactions chronologically into immutable blocks without executing chaincode.
  3. **Validation**: All Committing Peers validate endorsements against channel policy, check for Read-Write conflicts, and append block to the ledger.

---

# 🎯 High-Yield VTU Exam Solved Questions (10-Mark Model Answers)

### Question 1: Merkle Root Computation Numerical (10 Marks)
**Given 4 transactions with raw hashes:**
* $H_1 = \text{"AA"}$
* $H_2 = \text{"BB"}$
* $H_3 = \text{"CC"}$
* $H_4 = \text{"DD"}$

Illustrate the construction of the Merkle Tree and explain how a lightweight SPV client verifies that $Tx_2$ exists in the block using the Merkle Path.

**Solution:**
1. **Compute Level 1 Hashes**:
   * $H_{12} = \text{Hash}(H_1 + H_2) = \text{Hash}(\text{"AABB"})$
   * $H_{34} = \text{Hash}(H_3 + H_4) = \text{Hash}(\text{"CCDD"})$
2. **Compute Merkle Root**:
   * $\text{Root } H_{1234} = \text{Hash}(H_{12} + H_{34}) = \text{Hash}(\text{Hash}("AABB") + \text{Hash}("CCDD"))$
3. **SPV Proof for $Tx_2$**:
   * To verify $Tx_2$, the client only requires:
     1. The hash of transaction $Tx_2$ ($H_2$)
     2. Sister hash $H_1$
     3. Uncle hash $H_{34}$
     4. Expected Merkle Root $H_{1234}$ from the block header
   * Verification step: Client computes $H'_{12} = \text{Hash}(H_1 + H_2)$, then computes $\text{Root}' = \text{Hash}(H'_{12} + H_{34})$.
   * If $\text{Root}' == H_{1234}$, the transaction is mathematically authenticated without knowing $H_3$ or $H_4$.

---

### Question 2: Byzantine Generals Problem & 51% Attack (10 Marks)
* Define the Byzantine Generals Problem in distributed computing and describe how a 51% Attack is mounted on a Proof-of-Work blockchain network.

**Model Answer:**
1. **The Byzantine Generals Problem**:
   * A group of generals surrounding an enemy city must agree on a synchronized plan (Attack or Retreat).
   * Communicating only by messenger, some generals or messengers may be traitors trying to sow confusion.
   * Lamport proved that with $m$ traitors, consensus is impossible unless more than $2/3$ of the nodes are loyal ($N \ge 3m + 1$).
   * Satoshi Nakamoto solved this in Bitcoin using Proof-of-Work + Longest Chain Rule: nodes cast votes using computational hash power, making betrayal economically disastrous.
2. **51% Attack**:
   * If an attacker gains control of $>50\%$ of the network's total hash rate, they can produce blocks faster than the honest network.
   * **Mechanism**:
     1. Attacker spends tokens on the public chain (e.g. depositing onto a crypto exchange).
     2. Secretly mines an alternative private branch of blocks excluding the deposit transaction.
     3. Once the exchange credits the tokens, attacker broadcasts their longer private branch.
     4. The network nodes adopt the longer branch (Reorganization), effectively reversing the spending transaction (Double Spend).
