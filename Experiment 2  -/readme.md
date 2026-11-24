# Experiment 2: SHA-256 Data Integrity

## Overview

This experiment demonstrates how the SHA-256 hashing algorithm can be used to verify data integrity using Python. The program generates hashes for user-provided text and files and detects file modifications by comparing the original and modified hash values.

It uses Python's built-in `hashlib` module to generate SHA-256 hashes.

## Aim

To generate SHA-256 hashes for text and files and detect file modifications by comparing hash values.

## Objectives

- Understand the concept of cryptographic hashing.
- Generate SHA-256 hashes for text data.
- Generate SHA-256 hashes for files.
- Detect file modifications using hash comparison.
- Understand the importance of data integrity in cybersecurity.

## Methodology

1. Accept text or a file as input.
2. Generate its SHA-256 hash.
3. Display the file content and its hash.
4. Store the original hash before modification.
5. Append additional text to the file.
6. Generate the hash of the modified file.
7. Compare the original and modified hashes.
8. Display the integrity status.

## Implementation

The program uses Python's built-in `hashlib` module to generate SHA-256 hashes.

The program provides three main operations:

1. **Text Hashing:** Generate the SHA-256 hash of user-provided text.
2. **File Hashing:** Read a file and generate its SHA-256 hash.
3. **File Integrity Checking:** Append text to a file and compare its hash before and after modification.

## Features

- SHA-256 text hashing.
- SHA-256 file hashing.
- File content display.
- File modification through text appending.
- Original and modified hash comparison.
- Automatic integrity status.
- File-not-found handling.
- Menu-driven interface.

## Technologies Used

- **Programming Language:** Python
- **Module:** hashlib
- **Concepts:** Cryptographic Hashing, SHA-256, Data Integrity, File Handling.

## How to Run

**Step 1:** Clone the repository.

```bash
git clone <repository-url>
```

**Step 2:** Open the project folder.

```bash
cd SHA-256-Data-Integrity
```

**Step 3:** Run the Python program.

```bash
python main.py
```

**Step 4:** Select an operation from the menu and provide the required text or file path.

## Results

The program successfully generated SHA-256 hashes for text and files. After appending additional text to the file, the modified hash was different from the original hash, indicating that the file content had changed.

| Operation | Result |
|---|---|
| Text hashing | SHA-256 hash generated |
| File hashing | SHA-256 hash generated |
| Text appended | File modified successfully |
| Hash comparison | Change detected |

## Discussion

SHA-256 is a cryptographic hashing algorithm that produces a fixed-length **256-bit hash**, represented by 64 hexadecimal characters.

Even a small change in the original data produces a significantly different hash value. This property helps detect unauthorized or accidental modifications to files.

Hashing verifies data integrity but does not provide data confidentiality because it does not encrypt the original information.

## Improvements

1. Used a common hash-generation function for both text and file data.
2. Added automatic calculation of the original file hash.
3. Added automatic calculation of the modified file hash.
4. Implemented direct hash comparison after modification.
5. Limited the tampering simulation to appending text.
6. Added automatic `MODIFIED` or `UNCHANGED` status.
7. Added file-not-found handling.

Successfully implemented SHA-256 hashing using Python. The experiment demonstrated how cryptographic hashes can be used to verify data integrity and detect file modifications. It also helped in understanding the importance of hashing in cybersecurity applications such as file verification, digital forensics, and data protection.

**Note:** SHA-256 is a one-way hashing algorithm, not an encryption algorithm. It is commonly used for integrity verification and digital security.
