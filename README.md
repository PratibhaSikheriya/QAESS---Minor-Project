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

## ⚙️ System Workflow

1️⃣ Generate composite state containing both normal & covert messages  
2️⃣ Share among first generation using `(p,p)` QTSS  
3️⃣ When new parties join → modify + redistribute existing shares  
4️⃣ Reconstruction requires **all current parties** → prevents spy insertion  
5️⃣ Covert secret requires anamorphic keys `(k1, k2, n2, n4)` to recover

### The covert secret remains:
- 🔒 undetectable  
- 🕵 indistinguishable  
- 🧾 plausibly deniable  

---

## 🛠 Implementation Overview

| Module | Responsibility |
|--------|---------------|
| `QECC.py` | Quantum error-correcting code encoder/decoder |
| `TrapCode.py` | Embeds and verifies covert message inside trap structure |
| `Redistribution.py` | (p,p) QTSS redistribution for new party arrival |
| `Dealer.py` | Handles generations, share allocation & threshold evolution |
| `Reconstruction.py` | Normal & covert secret recovery routines |

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

🌱 Future Work

Improve fidelity using advanced QECC

NISQ-hardware execution benchmarking

Hybrid computational + information-theoretic QAESS

Trap-optimization for resource-efficient scaling
