# Experiment 1: Classical Symmetric Ciphers

## Overview

This experiment implements two classical symmetric encryption algorithms, **Caesar Cipher** and **Vigenère Cipher**, using Python. The program provides encryption and decryption functionality through a simple menu-driven interface.

It demonstrates how plaintext can be converted into ciphertext and how the original message can be recovered using the correct key.

## Aim

To implement Caesar Cipher and Vigenère Cipher in Python for encryption and decryption.

## Objectives

- Understand the basic concepts of symmetric encryption.
- Implement Caesar Cipher using a user-defined shift value.
- Implement Vigenère Cipher using a user-defined keyword.
- Perform encryption and decryption of messages.
- Understand the difference between substitution-based encryption techniques.

## Methodology

### 1. Caesar Cipher

1. Accept plaintext and shift value from the user.
2. Shift each alphabetic character using modulo 26.
3. Preserve uppercase and lowercase characters.
4. Keep spaces and special characters unchanged.
5. Decrypt the ciphertext by applying the negative shift.

### 2. Vigenère Cipher

1. Accept plaintext and an alphabetic key from the user.
2. Convert each key character into a numerical shift value.
3. Repeat the key throughout the plaintext.
4. Apply the key only to alphabetic characters.
5. Preserve spaces, punctuation, and character case.
6. Decrypt by subtracting the same key shifts.

## Features

- Menu-driven interface.
- Caesar Cipher encryption and decryption.
- Vigenère Cipher encryption and decryption.
- User-defined shift value.
- User-defined encryption key.
- Preserves uppercase and lowercase characters.
- Preserves spaces and special characters.
- Correct Vigenère key alignment across spaces.
- Automatic encryption and decryption verification.

## Technologies Used

- **Programming Language:** Python
- **Concepts:** Symmetric Encryption, Substitution Cipher, String Manipulation, Modulo Operation.

## How to Run

**Step 1:** Clone the repository.

```bash
git clone <repository-url>
```

**Step 2:** Open the project folder.

```bash
cd Classical-Symmetric-Ciphers
```

**Step 3:** Run the Python program.

```bash
python main.py
```

**Step 4:** Select an option from the menu and enter the required plaintext, shift value, or key.

## Results

Both encryption algorithms successfully performed encryption and decryption. The decrypted text matched the original plaintext in the tested cases.

| Cipher | Encryption | Decryption | Verification |
|---|---|---|---|
| Caesar Cipher | Successful | Successful | Passed |
| Vigenère Cipher | Successful | Successful | Passed |

## Discussion

**Caesar Cipher:** It uses a fixed shift value to replace each letter with another letter. It has a small key space, making it vulnerable to brute-force attacks.

**Vigenère Cipher:** It uses a repeating keyword to apply multiple shifts to the plaintext. It provides better protection against simple frequency analysis than Caesar Cipher. However, repeating-key patterns can still be exploited.

## Improvements

1. Added a menu-driven interface for easy interaction.
2. Implemented runtime input for plaintext, shift value, and encryption key.
3. Added automatic encryption and decryption verification.
4. Preserved uppercase and lowercase characters.
5. Preserved spaces and special characters.
6. Used a separate key index for correct Vigenère key alignment.

