🧬 Quantum Anamorphic Evolving Dynamic-Threshold Secret-Sharing (QAESS-SR)
⚡ Construction II — Implemented with Secret Redistribution

This repository contains an implementation of Construction II from the research paper
📄 “Construction of Quantum Anamorphic Evolving Dynamic-Threshold Secret-Sharing Schemes (QAESS)” 

Construction_of_Quantum_Anamorp…

.

The implemented version focuses on secret redistribution, resulting in low time complexity, reduced share size, and high feasibility — suitable for practical quantum-secure applications.

🔍 What is QAESS-SR?

QAESS-SR is a quantum secret-sharing scheme that provides:

Property	Description
Evolving	New parties can join the system over time without restarting the protocol
Dynamic-Threshold	At time t, the threshold is k(t) depending on the number of parties
Anamorphic	A covert secret is hidden inside the normal secret, detectable only by holders of covert keys
Secret Redistribution	Shares of previous parties are redistributed to new parties — reducing memory and execution cost

➡️ In simple words: The scheme shares a quantum secret in a network where participants join over time, supports a second covert message, and ensures no spy can detect the hidden layer.

🌟 Why Construction II?

Construction I of QAESS has two limitations:

Drawback	Reason
Very large share size	Shares grow using product over all generation combinations
Very high memory requirement	Dealer must store many intermediate states

Construction II solves this using Secret Redistribution, which only modifies shares once per party, keeping sizes minimal.

📌 From the research paper:

“…using secret redistribution significantly reduces the dimension of a party, and in this case, the dimension is bounded by O(I) even after insertion of covert messages…” 

Construction_of_Quantum_Anamorp…

🧠 Intuition Behind the Scheme
Normal Secret S  ——► Shared among parties  
Covert Secret Ŝ ——► Stealthily inserted using trap code (without increasing qubit count)


Whenever a new participant joins:

Old share of previous party (QECC encoded + traps + permutation + Pauli)  
       └─ redistributed to new party via (p,p)-QTSS


Thus every generation retains secrecy and trap-based anamorphic covert messaging, but with low overhead.

⚙️ System Workflow

1️⃣ Generate composite state containing both messages
2️⃣ Distribute shares of first generation using (p,p)-QTSS
3️⃣ When new parties arrive → modify + redistribute shares instead of recomputing
4️⃣ Reconstruction requires all current parties → prevents spy insertion attack
5️⃣ Classical keys (k1, k2, n2, n4) required only for covert reconstruction

🔐 The covert message remains:

undetectable,

indistinguishable,

plausibly deniable.

🛠 Implementation Overview
Module	Responsibility
QECC.py	Quantum error-correcting code encoder/decoder
TrapCode.py	Embeds and verifies covert information
Redistribution.py	(p,p) QTSS redistribution per new party
Dealer.py	Overall controller for generations and thresholds
Reconstruction.py	Normal & covert secret recovery
⏱ Time Complexity & Memory
Feature	Construction I	Construction II (Your implementation)
Share size growth	O(I⁴ log I (log log I)²)	O(I) ✔
Space complexity	Exponential	Linear ✔
Practical feasibility	❌ Very low	✔ High

➡️ Ideal for real-world deployment on quantum networks and post-quantum secure IoT environments.

🧾 Reconstruction Modes
Mode	What you recover
Normal	Only normal secret S
Anamorphic	Both S and covert secret Ŝ (requires anamorphic keys k1,k2,n2,n4)
Deniable	Reconstruction proves only S, without exposure of Ŝ
🔐 Security Guarantees
Property	Achieved By
Privacy	QTSS + semi-QTSS
Covert secrecy	Trap code layered permutation & Pauli masking
Indistinguishability	Covert insertion does not increase qubit dimension 

Construction_of_Quantum_Anamorp…


Plausible deniability	Adversary cannot detect anamorphic key presence 

Construction_of_Quantum_Anamorp…


No spy insertion	(∞,∞) threshold — every current party is required for reconstruction 

Construction_of_Quantum_Anamorp…

🚀 How to Run
# clone repo
git clone https://github.com/<user>/<repo>.git
cd <repo>

# install required libraries
pip install -r requirements.txt

# run simulation
python main.py


Expected output in simulation mode:

[+] Normal Secret reconstructed successfully.
[+] Covert Secret reconstructed (anamorphic mode).
[+] Indistinguishability maintained — no anomaly detected.

📌 Future Enhancements

Support for multiple covert messages using separate trap permutations

Hardware-realistic implementation using Qiskit / Cirq backends

Performance benchmarking across network topologies

🧑‍🤝‍🧑 Contributors

Project developed by Pratibha Sikheriya & Sree Lakshmi
as part of QAESS Minor Project 2025
