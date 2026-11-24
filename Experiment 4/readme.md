# Experiment 4 — X.509 Self-Signed Digital Certificate

**Course:** COM-511 Network Security Lab

## Aim

To generate an RSA private key and a self-signed X.509 digital certificate using OpenSSL, inspect its details, verify its authenticity, and confirm that the public key in the certificate matches the generated private key.

## Overview

An X.509 digital certificate is used to associate a public key with an identity. It contains information such as the subject, issuer, validity period, public key, and digital signature.

In this experiment, OpenSSL is used to generate a 2048-bit RSA private key and create a self-signed certificate. The certificate is signed using its own private key instead of being issued by an external Certificate Authority (CA).

The experiment also demonstrates certificate inspection, verification, fingerprint generation, and public-private key matching.

## Tools and Technologies

- **OpenSSL:** For key generation and certificate management.
- **RSA Algorithm:** For generating the public-private key pair.
- **X.509:** Digital certificate standard.
- **SHA-256:** For hashing and certificate fingerprint generation.
- **Linux Terminal:** For executing OpenSSL commands.

## Methodology

### Step 1: Setup Environment

- Checked the installed OpenSSL version.
- Created a separate working directory named `x509_lab`.
- Used this directory to store generated keys and certificates.

### Step 2: Generate RSA Private Key

- Generated a 2048-bit RSA private key using OpenSSL.
- Checked the validity of the generated key.
- Set restricted file permissions to protect the private key.

### Step 3: Create Self-Signed Certificate

- Generated an X.509 certificate using the RSA private key.
- Configured certificate details such as country, state, organization, and common name.
- Added Subject Alternative Name (SAN) entries for `localhost` and `127.0.0.1`.
- Set the certificate validity period to 365 days.
- Signed the certificate using its own private key.

### Step 4: Inspect Certificate Details

- Displayed the complete certificate information.
- Checked the subject and issuer details.
- Verified the validity dates and serial number.
- Inspected certificate extensions such as key usage and extended key usage.
- Generated the SHA-256 certificate fingerprint.

### Step 5: Verify the Certificate

- Verified the certificate without specifying a trusted CA.
- Observed that verification failed because the certificate was self-signed and not trusted by default.
- Verified the certificate again by explicitly providing it as a trusted CA file.
- Received the successful verification result.

### Step 6: Verify Private and Public Key Matching

- Extracted the public key from the private key.
- Extracted the public key from the generated certificate.
- Generated SHA-256 hashes of both public keys.
- Compared the hashes to confirm that both public keys matched.

### Step 7: Optional PEM to DER Conversion

- Converted the certificate from PEM format to DER format.
- Inspected the converted certificate.
- Confirmed that both formats represented the same certificate.

## Files Generated

| File | Description |
|---|---|
| `private.key` | RSA private key used for signing the certificate. |
| `certificate.crt` | Self-signed X.509 certificate in PEM format. |
| `certificate.der` | Same certificate in binary DER format. |

**Security Note:** The private key must never be uploaded to GitHub or shared publicly. Only public certificate files should be distributed.

## Results and Observations

1. Successfully generated a 2048-bit RSA private key.
2. Created a self-signed X.509 certificate using OpenSSL.
3. Observed that the subject and issuer fields were identical.
4. Successfully inspected the certificate details and extensions.
5. Certificate verification failed by default because it was not trusted by the system.
6. Verification returned `OK` when the certificate was explicitly provided as a trust anchor.
7. Both SHA-256 public key hashes matched, confirming that the certificate and private key belonged to the same key pair.
8. Successfully converted the certificate from PEM to DER format.

