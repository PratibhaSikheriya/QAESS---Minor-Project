# 🧬 Quantum Anamorphic Evolving Dynamic-Threshold Secret-Sharing (QAESS-SR)  
### ⚡ Construction II — Implemented with Secret Redistribution

This repository contains an implementation of **Construction II** from the research paper:  
📄 *“Construction of Quantum Anamorphic Evolving Dynamic-Threshold Secret-Sharing Schemes (QAESS)”*  
with a focus on **secret redistribution**, ensuring **low time complexity**, **reduced share size**, and **practical real-world feasibility**.

---

## 🔍 What is QAESS-SR?

QAESS-SR is a **quantum secret-sharing scheme** that supports evolving participation and covert communication.

| Property | Description |
|---------|-------------|
| **Evolving** | New parties join over time without restarting the protocol |
| **Dynamic-Threshold** | Threshold at time `t` is `k(t)` depending on the number of parties |
| **Anamorphic** | A hidden covert secret is embedded inside the normal secret |
| **Secret Redistribution** | Shares of previous parties are redistributed to new ones — avoiding recomputation |

➤ **In simple words:** A quantum secret is shared among a growing network of participants, and a second *hidden* secret is embedded without being detectable — even under adversarial inspection.

---

## 🌟 Why Construction II?

Construction I of QAESS has two technical limitations:

| Drawback | Reason |
|---------|--------|
| Extremely large share size | Grows over all combinations of generations |
| High memory requirement | Dealer must store multiple intermediate states |

Construction II solves both problems using **Secret Redistribution**, which modifies each share **only once**, keeping share sizes small.

> “Using secret redistribution significantly reduces the dimension of a party, and the dimension becomes O(I) even after insertion of covert messages.”

---

## 🧠 Intuition Behind the Scheme

Normal Secret S ──► Shared using QTSS
Covert Secret Ŝ ──► Embedded inside trap structure via permutation + Pauli masking

Whenever a new participant joins:

Old share of previous party (QECC encoded + traps + permutation + Pauli)
└─ redistributed to new party via (p,p)-QTSS

Thus, every generation keeps:
✔ secrecy  
✔ covert messaging  
✔ low computational cost

---

## Key Features

- ✅ Steane [[7,1,3]] quantum error correction
- ✅ Construction 2 trap constraint: n1⊕n2 = n3⊕n4
- ✅ GF(2^13) threshold secret sharing
- ✅ SHA-512 commitment scheme
- ✅ Multi-generation redistribution
- ✅ Covert channel encoding
- ✅ Quantum attack detection

-- Pipeline: Algo 6 (Distribute) → Algo 2 (Verify) → Algo 7 (Reconstruct)

## ⚙️ System Workflow

1️⃣ Generate composite state containing both normal & covert messages  
2️⃣ Share among first generation using `(p,p)` QTSS  
3️⃣ When new parties join → modify + redistribute existing shares  
4️⃣ Reconstruction requires **all current parties** → prevents spy insertion  
5️⃣ Covert secret requires anamorphic keys `(k1, k2, n2, n4)` to recover


## Deatiled

# Algorithm 6: Secret Distribution & Encoding
- Setup

- Inputs: Normal secret S (8 bits), Covert secret Sc (6 bits)
- Keys: k1 (permutation), k2 (Pauli operations) in GF(2^13)
- Qubits: 23 total = 7 (Steane) + 8 (traps) + 6 (covert) + 2 (random Pauli)

# Core Steps

- Encode S[0] → Steane [[7,1,3]] QECC (qubits 0-6)
- Encode S[1:8] → Trap qubits n1,n2,n3,n4 (qubits 7-14)
- Encode Sc → Covert channel (qubits 15-20)
- Apply → Superposition, Entanglement, Permutation(k1), Pauli(k2)
- Share keys → GF(2^13) Shamir secret sharing
- Create commitments → SHA-512 for verification

# Outputs

- Quantum states for each generation
- Key shares (k1, k2, n2, n4)
- SHA-512 commitments
- Measurement distributions


# Algorithm 6.2: Verification Protocol
- Phase 0: Attack Detection

- PNS Attack: Check trap violation rates
- MITM Attack: Validate state integrity
- Trojan Attack: Detect dimensional anomalies
- Intercept-Resend: Measure basis correlation

# Procedure 2a: Dealer-Message Verification

- Apply P^(-1)_k2 (inverse Pauli)
- Apply σ^(-1)_k1 (inverse permutation)
- Discard trap qubits
- Decode Steane → Recover message
- Verify SHA-512 commitment

 # Procedure 2b: Party-Key Verification

- Reconstruct k1, k2, n2, n4 from GF(2^13) shares
- Verify SHA-512 commitments for all keys
- Shannon entropy test (≥0.75 threshold)


# Algorithm 7: Secret Reconstruction
- Normal Secret (Algorithm 3)

- Apply P^(-1)_k2 → Undo Pauli encryption
- Apply σ^(-1)_k1 → Undo permutation
- Discard traps → Keep encoded qubits
- Decode QECC → Measure with 1024 shots
- Calculate fidelity vs original

# Covert Secret (Algorithm 4)

- Extract trap structure: n2 zeros + n4 ones
- Reconstruct from permutation pattern
- Calculate fidelity vs original

# Fidelity Analysis

- Target: ≥70% for both secrets
- Metrics: Bit-matching fidelity, Hamming distance
- Visualization: Comparison graphs across input types

### The covert secret remains:
- 🔒 undetectable  
- 🕵 indistinguishable  
- 🧾 plausibly deniable  

---

## ⏱ Time Complexity & Memory

| Feature | Construction I | Construction II (This repo) |
|--------|---------------|-----------------------------|
| Share size growth | `O(I⁴ log I (log log I)²)` | `O(I)` ✔ |
| Space complexity | Exponential | Linear ✔ |
| Practical feasibility | ❌ Very low | ✔ High |

➡️ Suitable for **Quantum Networks, Quantum Internet, and Post-Quantum IoT security applications**

---

## 🔐 Reconstruction Modes

| Mode | Output |
|------|--------|
| **Normal Mode** | Recovers only the normal secret `S` |
| **Anamorphic Mode** | Recovers both `S` and covert secret `Ŝ` *(requires keys: `k1, k2, n2, n4`)* |
| **Deniable Mode** | Reconstructs only `S` — adversary cannot detect Ŝ even if inspected |

---

## 🔐 Security Guarantees

| Property | Achieved Through |
|---------|------------------|
| Privacy | QTSS + Semi-QTSS |
| Covert secrecy | Trap code + permutation + Pauli masking |
| Indistinguishability | Covert embedding **does NOT increase qubit count** |
| Plausible deniability | Adversary cannot detect presence of covert key |
| Spy attack resistance | `(∞,∞)` threshold → **all current parties required for reconstruction** |

---

## 🌱 Future Work

- Improve fidelity using advanced QECC
- NISQ-hardware execution benchmarking
- Hybrid computational + information-theoretic QAESS
- Trap-optimization for resource-efficient scaling

---

## ✨ Contributors

- Pratibha Sikheriya 
- Eluri Sree Lakshmi
- Supervision — Dr. Rajesh Doriya	
