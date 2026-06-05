# Precision Loss Vulnerability: Deep Dive & Fuzzing PoC

*If this research helped you, please consider giving it a ⭐ Star.*


## 🚀 Stay Updated
Found this research useful?
* **Star ⭐** this repo to keep track of it.
* **Follow me** on GitHub for more DeFi security research.
* **Fork** it if you want to run your own experiments.

### ☕ Support the Research
If you appreciate the work and want to support further security research:

<img src="456.PNG" alt="Donate QR" width="200"/>

**Wallet Address (ETH/EVM):** 0xBDDD7973D0DE27B715A4A5cbdb87d0DF78757b3A 


This repository contains a comprehensive security analysis and a production-ready **Proof of Concept (PoC)** for a Critical-level Precision Loss vulnerability identified in DeFi protocols (Kuru Labs Case Study).

## 🛡️ Executive Summary
In decentralized finance, the order of operations in mathematical formulas is critical. This project demonstrates how premature division leads to significant precision loss, often resulting in **100% loss of user funds** (the "Zero-Share" or "Zero-Output" attack vector).

### Key Vulnerability: Premature Division
Standard Solidity integer division truncates the remainder. When a formula performs division before multiplication, the intermediate result can drop below 1, causing the entire subsequent calculation to collapse to zero.

**Vulnerable Pattern:** `(amount * price) / base * multiplier / base`
**Correct Pattern:** `(amount * price * multiplier) / (base * base)`

## 🚀 Proof of Concept (Foundry)
The repository includes a specialized test suite built with **Foundry** that uses both static and fuzz testing to identify edge cases.

### Running the Tests
To verify the vulnerability, run:
```bash
forge test -vv
Fuzzing Strategy
The testPrecisionLossFuzz function utilizes property-based testing to find specific values of p (price), s (shares), and mult (multiplier) where the bugged formula returns 0 while the correct mathematical result is significantly higher.

Example Found by Fuzzer:

Input: p: 1e7, s: 3.79e9, mult: 1e25

Bugged Result: 0

Correct Result: 379,247

Impact: Critical loss of assets for the end-user.

📊 Technical Confirmation
This vulnerability was analyzed during the Kuru Labs Audit, where it was confirmed by judges with the following metrics:

Severity: Critical

Likelihood: High

Impact: High

🛠 Tools Used
Foundry / Forge: For advanced fuzzing and unit testing.

Solidity 0.8.x: Core smart contract logic.

Developed for security research purposes. Part of the rdin777 audit portfolio.
# kuru-precision-loss
