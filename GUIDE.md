# HashGuard — Complete Project Implementation Guide
**Crack-Proof? An Interactive Study on Hash Collisions and Password Entropy**

Submitted by: Sujata Pareek · Ayushi Dahiya · Vishwamitra Sabharwal
Department of ECE — JIIT Noida | March 2026

---

## ⚠️ Important Clarification About the GitHub Repo

The GitHub link `Liuql23/WatermarkSchemeBasedonBlockchian` your faculty shared is from a **different research paper topic** (Digital Watermarking using Blockchain — MATLAB-based). It is **not related to your HashGuard synopsis**. Your project is entirely Python-based. This guide builds your project from scratch, exactly as described in your synopsis.

---

## 📁 Project Folder Structure

```
HashGuard/
├── hashguard.py       ← Main program (all 5 modules inside)
├── passwords.txt      ← Wordlist for dictionary attack
├── requirements.txt   ← Python dependencies
└── GUIDE.md           ← This file
```

---

## 🖥️ STEP 1 — Setting Up Your Environment

### What you need installed:
- **Python 3.10+** → Download from https://python.org (tick "Add to PATH" during install on Windows)
- **VS Code** (recommended editor) → https://code.visualstudio.com

### Verify Python is installed:
Open **Command Prompt** (Windows) or **Terminal** (Mac/Linux) and type:
```bash
python --version
```
You should see something like `Python 3.11.x`

### Install the only required library (colorama for colored output):
```bash
pip install colorama
```

---

## 📂 STEP 2 — Create the Project Folder

1. Create a new folder called `HashGuard` anywhere on your computer (e.g., Desktop)
2. Inside it, create two files:
   - `hashguard.py` (copy the full code given)
   - `passwords.txt` (copy the wordlist given)

---

## 🔧 STEP 3 — Understanding Each Module (Where & How It's Implemented)

All 5 modules live inside `hashguard.py`. Here's what each one does and where to find it:

---

### MODULE 1 — Hashing Engine
**Location:** Functions `hash_password()`, `show_algorithm_comparison()`, `demonstrate_avalanche()`
**Lines:** ~55–95 in hashguard.py

**What it does:**
- Takes a user's password string
- Encodes it to UTF-8 bytes
- Applies `hashlib.sha256()` / `hashlib.md5()` / `hashlib.sha1()`
- Returns a fixed-length hexadecimal digest

**How it works (step by step):**
```python
import hashlib
password = "hello"
encoded  = password.encode("utf-8")          # b'hello'
digest   = hashlib.sha256(encoded).hexdigest()
# → '2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824'
```

**Avalanche Effect Demo:** Changing just one character ("hello" → "iello") produces a completely different 64-character hash — this is demonstrated in `demonstrate_avalanche()`.

---

### MODULE 2 — Entropy & Complexity Analyzer
**Location:** Functions `analyze_complexity()`, `print_complexity_report()`
**Lines:** ~100–160 in hashguard.py

**What it does:**
- Checks for 4 character sets: lowercase (26), UPPERCASE (26), digits (10), symbols (32)
- Calculates: `entropy_bits = L × log₂(S)` where L=length, S=charset size
- Assigns a grade: Weak / Medium / Strong / Excellent

**Grade thresholds:**
| Entropy (bits) | Grade     |
|----------------|-----------|
| < 28           | WEAK      |
| 28 – 49        | MEDIUM    |
| 50 – 74        | STRONG    |
| 75+            | EXCELLENT |

**Example:**
- "password" → S=26, L=8 → entropy = 8 × log₂(26) ≈ 37.6 bits → **MEDIUM**
- "P@ssw0rd#2024!" → S=94, L=14 → entropy = 14 × log₂(94) ≈ 92 bits → **EXCELLENT**

---

### MODULE 3 — Time-to-Crack Estimation
**Location:** Functions `estimate_crack_time()`, `format_time()`
**Lines:** ~163–195 in hashguard.py

**What it does:**
Estimates how long brute-force would take under 3 real-world scenarios:
1. Online attack (rate-limited): 1,000 hashes/sec
2. Offline MD5 (GPU): 10 billion hashes/sec
3. Offline SHA-256 (GPU): 1 billion hashes/sec

**Formula:** `time_in_seconds = total_combinations / hashes_per_second`

---

### MODULE 4 — Dictionary Attack Simulator
**Location:** Function `dictionary_attack()`
**Lines:** ~198–240 in hashguard.py

**What it does:**
1. Opens `passwords.txt` (the wordlist)
2. For each word → computes its SHA-256 hash
3. Compares to the target hash
4. If matched → reports "CRACKED" + the original password

**This simulates a real attacker's workflow.** For example, the hash of "password" will be found in under 0.1 seconds.

**Where to put `passwords.txt`:** In the **same folder** as `hashguard.py`

**Want a bigger wordlist?** Download rockyou.txt (~14 million passwords) from:
`https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt`
Then change the `WORDLIST` path in `main()` to point to it.

---

### MODULE 5 — Salt & Pepper Defense
**Location:** Functions `generate_salt()`, `hash_with_salt()`, `hash_with_salt_and_pepper()`, `demonstrate_salting()`
**Lines:** ~243–290 in hashguard.py

**What it does:**
- **Salt:** A unique random string added per-user before hashing → defeats Rainbow Tables
- **Pepper:** A global server-side secret added additionally → even a leaked DB can't be cracked

**Demo shows:**
- Same password hashed twice WITHOUT salt → SAME hash (vulnerable)
- Same password hashed twice WITH different salts → DIFFERENT hashes (safe)

```
Without salt: "password" → 5e884898... (always same — Rainbow Tables work!)
With salt:    "password" + salt1 → 9a3b2f1c... (unique per user!)
              "password" + salt2 → f4d81e7a... (different every time!)
```

---

## ▶️ STEP 4 — How to Run the Project

Open terminal, navigate to your HashGuard folder:

```bash
cd Desktop/HashGuard
python hashguard.py
```

You'll see the main menu:
```
  MENU:
   1. Analyze a password (full report)     ← Runs ALL 5 modules
   2. Hash a custom password               ← Module 1 only
   3. Run dictionary attack on a hash      ← Module 4 only
   4. Demonstrate Salt & Pepper defense    ← Module 5 only
   5. Quit
```

**For your project demo, choose option 1** and enter these test passwords to show the full range:
- `password` → Shows as MEDIUM, gets cracked immediately by dictionary
- `abc` → Shows as WEAK, gets cracked
- `P@ssw0rd#JIIT2024!` → Shows as EXCELLENT, survives the dictionary attack

---

## 🧪 STEP 5 — Testing Your Project (For Viva)

Run this to verify all modules work:
```bash
python -c "
import hashguard
# Test hashing
assert len(hashguard.hash_password('test')) == 64
# Test complexity
assert hashguard.analyze_complexity('abc')['grade'] == 'WEAK'
assert hashguard.analyze_complexity('P@ssw0rd#2024!')['grade'] in ('STRONG','EXCELLENT')
# Test salting
s = hashguard.generate_salt()
assert hashguard.hash_with_salt('pwd', s) == hashguard.hash_with_salt('pwd', s)
print('All tests passed!')
"
```

---

## ❓ Common Errors & Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `ModuleNotFoundError: colorama` | Library not installed | Run `pip install colorama` |
| `FileNotFoundError: passwords.txt` | Wordlist missing or wrong folder | Put `passwords.txt` in same folder as `hashguard.py` |
| `python: command not found` | Python not in PATH | Use `python3` instead of `python`, or reinstall Python with "Add to PATH" ticked |
| MATLAB errors from GitHub repo | Wrong project — that's a watermarking project | Ignore that repo; your project is Python-only |

---

## 📌 What to Tell Your Faculty

> "The GitHub repository `WatermarkSchemeBasedonBlockchian` contains MATLAB code for a blockchain-based digital watermarking scheme, which is a different topic from our synopsis. Our project, HashGuard, is implemented in Python using `hashlib`, `colorama`, and standard libraries, as specified in our synopsis. We have implemented all 5 components: the hashing engine, complexity analyzer, time-to-crack estimator, dictionary attack simulator, and salt/pepper defense."

---

## 📚 References (As in Synopsis)

1. NIST Special Publication 800-63B — Digital Identity Guidelines
2. Python `hashlib` Documentation — docs.python.org/3/library/hashlib.html
3. "Serious Cryptography" by Jean-Philippe Aumasson
4. OWASP Password Storage Cheat Sheet — owasp.org
