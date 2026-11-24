# Experiment 8 — Protected Email Exchange with OpenPGP and Thunderbird

## 1. Aim

To perform a secure two-way email exchange using OpenPGP in Mozilla Thunderbird by encrypting email messages, decrypting received messages, and verifying digital signatures to ensure confidentiality, integrity, and sender authentication.

## 2. Overview

OpenPGP (Open Pretty Good Privacy) is a standard used to secure digital communication through encryption and digital signatures.

It uses public-key cryptography to protect email messages. The sender encrypts a message using the recipient's public key, and the recipient decrypts it using their private key.

Digital signatures are used to verify message integrity and confirm that the message was signed by the private key corresponding to the sender's public key.

In this experiment, Mozilla Thunderbird was used to exchange encrypted and digitally signed emails between Kanishka and Tanvi in both directions.

## 3. Tools and Technologies

- **Mozilla Thunderbird:** Email client with built-in OpenPGP support.
- **OpenPGP:** Standard for email encryption and digital signatures.
- **Public-Key Cryptography:** Used for encryption and signature verification.
- **Private Key:** Used for decrypting messages and creating digital signatures.
- **Public Key:** Used for encrypting messages and verifying signatures.
- **Digital Signature:** Used to verify message integrity and sender-key association.

## 4. Account and Key Details

| Participant | Email Address / Name | Key ID |
|---|---|---|
| Kanishka | `2024a7r052@mietjammu.in` | `0x12DE1760A3ABF8CA` |
| Correspondent | Tanvi | `0xC0C83C39934244E2` |

**Note:** Key IDs identify the OpenPGP keys but do not independently verify the real identity of their owners. Full fingerprints should be verified through a trusted communication channel.

## 5. Theory

OpenPGP provides two important security features:

### A. Email Encryption

- The sender uses the recipient's public key to encrypt the email.
- The encrypted message can only be decrypted using the corresponding private key.
- This protects message confidentiality.

### B. Digital Signature

- The sender creates a digital signature using their private key.
- The recipient verifies the signature using the sender's public key.
- This helps verify message integrity and the association of the signature with the corresponding key.

### C. Security Services

| Security Feature | Purpose |
|---|---|
| Confidentiality | Prevents unauthorized users from reading email content. |
| Integrity | Helps detect unauthorized message modifications. |
| Authentication | Verifies that the message was signed by the private key associated with the checked public key. |
| Non-repudiation | Provides cryptographic evidence of signing, subject to private-key control and identity verification. |

## 6. Methodology

### Step 1: Configure Thunderbird Account

- Opened Mozilla Thunderbird.
- Selected the email account associated with Kanishka.
- Opened the account's OpenPGP security settings.
- Selected or configured the OpenPGP key pair for the account.

### Step 2: Exchange Public Keys

- Exported Kanishka's public key.
- Shared the public key with Tanvi.
- Received Tanvi's public key.
- Imported Tanvi's public key into Thunderbird.
- Checked the complete key fingerprint through a trusted communication channel before trusting the key.

### Step 3: Send Encrypted and Digitally Signed Email

- Composed a new email addressed to Tanvi.
- Enabled the message encryption option.
- Enabled the digital signature option.
- Sent the email using Kanishka's OpenPGP key.

The message was encrypted for Tanvi using her public key and signed using Kanishka's private key.

### Step 4: Receive and Verify the Email

- Tanvi received the encrypted email.
- Used her private key to decrypt the message.
- Thunderbird verified the digital signature using Kanishka's public key.
- Checked the message security details to confirm successful decryption and signature verification.

### Step 5: Send the Reply from Tanvi

- Tanvi composed a reply to Kanishka.
- Enabled OpenPGP encryption and digital signing.
- Encrypted the message using Kanishka's public key.
- Signed the message using Tanvi's private key.
- Sent the reply securely.

### Step 6: Receive and Verify the Reply

- Kanishka received the encrypted reply.
- Used her private key to decrypt the email.
- Thunderbird verified Tanvi's digital signature using her public key.
- Checked the security details to confirm successful decryption and a valid signature.

## 7. OpenPGP Email Exchange Flow

```text
             KANISHKA                     TANVI
                |                           |
                |                           |
                |<----- Public Key Exchange ->|
                |                           |
                |                           |
                | Encrypt using Tanvi's     |
                | public key                |
                | Sign using Kanishka's key |
                |                           |
                |-------- Encrypted Email ->|
                |                           |
                |                Decrypt using
                |                private key
                |                Verify signature
                |                           |
                |<------- Encrypted Reply --|
                |                           |
                | Decrypt using private key |
                | Verify Tanvi's signature  |
                |                           |
                +------ Exchange Complete --+
```

## 8. Results and Observations

The two-way OpenPGP email exchange was completed successfully using Mozilla Thunderbird.

| Message Direction | Decryption | Signature Verification | Associated Key IDs |
|---|---|---|---|
| Kanishka → Tanvi | Successful | Valid | Kanishka: `0x12DE1760A3ABF8CA` |
| Tanvi → Kanishka | Successful | Valid | Tanvi: `0xC0C83C39934244E2` |

### Key Observations

1. Kanishka successfully sent an encrypted and digitally signed email to Tanvi.
2. Tanvi successfully decrypted the received email using her private key.
3. Thunderbird displayed a valid signature for Kanishka's message.
4. Tanvi successfully sent an encrypted and digitally signed reply.
5. Kanishka successfully decrypted the reply using her private key.
6. Thunderbird displayed a valid signature for Tanvi's reply.
7. Both directions demonstrated successful encryption, decryption, and signature verification.

## 9. What the Results Demonstrate

- **Confidentiality:** Email contents were protected using recipient-specific public-key encryption.
- **Integrity:** Digital signature verification confirmed that the received messages matched their signatures.
- **Authentication:** The signatures were verified using the corresponding public keys.
- **Secure Communication:** Both participants successfully exchanged protected email messages.

**Important:** A valid signature confirms the relationship between a message and the signing key. The actual identity of the person controlling that key must be established through fingerprint verification.

## 10. Improvements

1. **Verify Full Fingerprints:** Always compare complete OpenPGP fingerprints through a trusted channel instead of relying only on short key IDs.
2. **Protect Private Keys:** Use strong passphrases and securely store private keys to prevent unauthorized access.
3. **Use Strong Algorithms:** Prefer modern OpenPGP-supported cryptographic algorithms and configurations.
4. **Regular Key Management:** Review key expiration dates and renew or replace keys when required.
5. **Secure Key Backup:** Maintain encrypted backups of private keys and revocation certificates.
6. **Verify Every Signature:** Check the signature status before trusting received messages.

## 11. Output and Evidence

The following evidence was recorded or should be included in the GitHub repository:

- Kanishka's sent-message security view showing encryption and a valid signature.
- Kanishka's received-reply security view showing successful decryption and a valid signature.
- Kanishka's OpenPGP key details.
- Tanvi's OpenPGP key details.
- Public-key exchange evidence.
- Full fingerprint verification evidence, if required by the experiment.

## 12. Screenshots

Add the following screenshots from the Thunderbird experiment:

1. Kanishka's OpenPGP key details.
2. Tanvi's imported public key details.
3. Kanishka's sent email showing encryption and digital signature.
4. Tanvi's received email showing successful decryption and valid signature.
5. Tanvi's encrypted and signed reply.
6. Kanishka's received reply showing successful decryption and valid signature.

## 13. Conclusion

Successfully performed a two-way secure email exchange using OpenPGP in Mozilla Thunderbird between Kanishka and Tanvi. Both participants exchanged encrypted and digitally signed email messages. The received messages were decrypted successfully, and Thunderbird displayed valid digital signatures in both directions.

This experiment provided practical knowledge of public-key cryptography, email encryption, digital signatures, key exchange, and secure email communication. It demonstrated how OpenPGP helps maintain confidentiality and message integrity during email communication.


- Understood digital signatures and signature verification.
- Learned how to encrypt and decrypt email messages.
- Understood confidentiality, integrity, and authentication in secure email communication.
