
# HashGuard: Password Security & Entropy Analyzer

HashGuard is a comprehensive study on password vulnerability. It generates detailed security reports by processing passwords through multiple cryptographic hashing algorithms and testing them against common attack vectors.

## 🚀 Core Features
* **Triple Hashing Engine:** Generates and compares digests for **MD5**, **SHA-1**, and **SHA-256**.
* **Entropy Analyzer:** Calculates the mathematical strength (bits) of a password.
* **Attack Simulator:** Performs a real-time dictionary attack to see if the password is "leak-ready."
* **Defense Demo:** Showcases how **Salting** and **Peppering** prevent Rainbow Table attacks.

## 🛠️ How it Works
When you input a password, the system generates a report including:
1. **The Hashes:**
    * **MD5:** 128-bit hash (fast, but cryptographically broken).
    * **SHA-1:** 160-bit hash (legacy, no longer secure for passwords).
    * **SHA-256:** 256-bit hash (industry standard for modern security).
2. **The Avalanche Effect:** Demonstrates how changing one letter changes the entire hash.
3. **Time-to-Crack:** Estimates how long it would take an attacker to brute-force the hash.

## 💻 Installation & Usage
1. Clone the repo: `git clone https://github.com/yourusername/hash-guard.git`
2. Install dependencies: `pip install colorama`
3. Run the tool: `python hashguard.py`
