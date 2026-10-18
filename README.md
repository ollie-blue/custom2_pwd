# Project: Linux Forensics and Reverse Engineering - FindAndFixMe-V3 Challenge

## 📋 Executive Summary

Completed an advanced cybersecurity forensics and reverse engineering challenge involving a disguised/corrupted file (`findAndFixMe-V3.pdf`). Inspected file headers, corrected ZIP magic bytes to extract a hidden Python script (`findMeV5.py`), analyzed Fernet symmetric encryption, and utilized custom deobfuscation and helper scripts (`beautified.py`) in VS Code to uncover the hidden password challenge solution (`CYBERPedii`).

---

## 🛠️ Architecture & Tech Stack

- **Operating System:** Ubuntu Linux / WSL / VS Code Environment
- **Core Language:** Python 3
- **Cryptography Library:** `cryptography.fernet` (Fernet symmetric encryption)
- **Analysis & Forensics Tools:** `file`, `strings`, `hexedit`, `unzip`, VS Code Terminal & Text Editor.

---

## ⚙️ Step-by-Step Implementation

### Step 1: File Forensics & Initial Inspection

- Inspected the provided `findAndFixMe-V3.pdf` file using Linux terminal utilities within VS Code to analyze its true file format and structure.
- Executing `file findAndFixMe-V3.pdf` revealed that despite the `.pdf` extension, the file was actually a ZIP archive data structure.
- Attempting to extract it via `unzip` returned a `bad zipfile offset (local header sig): 0` error due to header corruption. Running `strings` on the file exposed references to an embedded script named `findMeV5.py`.

````bash
# Example commands used during initial file forensics
file findAndFixMe-V3.pdf
unzip findAndFixMe-V3.pdf
strings findAndFixMe-V3.pdf
````


### Step 2: Correcting File Headers & Extracting the Hidden Script
- Opened the file in a hex editor (`hexedit findAndFixMe-V3.pdf`) to inspect the raw bytes.
- Identified that the header incorrectly combined PDF and ZIP markers. Updated the magic bytes to the standard ZIP signature (`50 4B 03 04` with PK header).
- Successfully extracted the contents to reveal the hidden script: `findMeV5.py`.

### Step 3: Script Analysis & Deobfuscation (`beautified.py`)
- Inspected `findMeV5.py` to find an ASCII text executable consisting of an extremely long single line containing obfuscated strings, Fernet keys (`bb`), and encrypted payloads (`b`).
- Created a helper script (`beautified.py`) to parse, inspect, and safely interact with the encrypted execution blocks.
- Leveraged the Python `cryptography.fernet` library to decrypt the inner logic, exposing a user input password prompt and conditional validation loops mapping back to an obfuscated character array (`muHa`).

```python
from cryptography.fernet import Fernet

# Fernet decryption key and encrypted payload handling from beautified.py
bb = b"EAeeFGJDUZEeaishu4qaxswNUmKcmTDn1HjrKYN8U7Y="
b = b"gAAAAABjDLCIjp7gZPdLEynEO0XaCNusDgM5jjUkKQ7hoXLGIQq4t5UoQw9WmsgAup3XlBkqkwWhgsQM7qYuj3piy0xpRfQnh4F6Pa6XOkXimrH9gjtQnphSnri_3q8Y6oshPIcz9rbgl1CVjCpEGEi3DPRS5Oe-CstC_CNQG-pDFmU255PeksQdMWNI2v9e3Ugy22hwB8ndgW6H3jNTZ89ru5DH-MUKLFMwcYEKQtQhC3XlAwJaOXwU_WZ5X9TMbV1ODBChm-taN8T2V0pU8hx9Yji5brVHfgUfXGcAdGZKGJItMpixYZ0ISvnBvHB04bWNwlf1eBMmG_3Jpl33SerrCCIe4OfOPpB2ktRbhDcepp16mlnwbfo75ZZAMNbqP3RRK2-eWjFEKY47GTf2pN6KMKW6V2s3nK9lZpP4FLPVJ16OsuPLd8Q6NrcKwt2o5sYPA54M3fb22-4U6hQhkmVgJ_m24SHRCw=="

exec(Fernet(bb).decrypt(b))
```

### Step 4: Verification & Solution Discovery
- Executed beautified.py in the VS Code terminal to run the decrypted validation challenge.
- Provided test inputs to the interactive PASS? prompt until matching the required character sequence stored in the obfuscated array.
Successfully discovered the correct target password: CYBERPedii, which returned the confirmation message yup!.


## 🧪 Testing and Validation
- Validated the complete forensic pipeline by confirming that raw byte modifications successfully repaired the corrupted archive structure.
- Confirmed that executing the deobfuscated Fernet-encrypted script correctly isolated the password verification logic and output the expected success status.

## 💡 Key Learnings & Challenges
- Challenge: Dealing with a disguised file header (findAndFixMe-V3.pdf) that masked a corrupted ZIP archive containing obfuscated Python source code.
- Solution: Used low-level inspection tools (file, strings, hexedit) to identify correct magic bytes, followed by structured script formatting and Fernet decryption analysis via beautified.py.
- Key Takeaway: Gained practical expertise combining digital forensics, file header reconstruction, reverse engineering, and Python cryptography handling to bypass multi-layered obfuscation.

## 🚀 Future Improvements
- Build an automated Python utility to scan files for abnormal headers and test standard magic byte signatures.
- Implement static analysis scripts to automatically unpack and inspect embedded payloads without requiring manual hex editing.