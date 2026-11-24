# Experiment 5 — TLS Handshake Capture and Analysis

## Aim

To capture and analyze an HTTPS connection using Wireshark and study the TLS handshake, negotiated security parameters, and server certificate exchanged between the client and server.

## Overview

Transport Layer Security (TLS) is a security protocol that provides encrypted communication between a client and a server. It helps protect sensitive information from unauthorized access during transmission.

In this experiment, an HTTPS connection was captured using Wireshark to understand the TLS 1.3 handshake process. Important handshake messages such as ClientHello and ServerHello were identified and analyzed.

The experiment also demonstrated how SSLKEYLOGFILE can be used to decrypt protected TLS handshake messages for detailed packet-level analysis.

## Tools and Technologies

- **Wireshark:** For capturing and analyzing network packets.
- **Web Browser (Firefox):** For establishing an HTTPS connection.
- **HTTPS:** Secure communication protocol.
- **TLS 1.3:** Protocol used for secure communication.
- **SSLKEYLOGFILE:** Used to provide session secrets for TLS decryption.
- **PCAPNG:** File format used to save captured network traffic.

## Objectives

- Capture HTTPS traffic over TCP port 443.
- Identify ClientHello and ServerHello messages.
- Examine the options proposed by the client.
- Analyze the security parameters selected by the server.
- Inspect the server's digital certificate.
- Understand encrypted handshake messages in TLS 1.3.
- Observe how TLS establishes a secure communication channel.

## Methodology

### Step 1: Select Network Interface

- Opened Wireshark.
- Selected the active network interface that was generating network traffic.
- Prepared the interface for HTTPS packet capture.

### Step 2: Capture HTTPS Packets

- Applied the following capture filter:

```bash
tcp port 443
```

- Opened a fresh private browser window.
- Accessed the website:

```text
https://www.wireshark.org/
```

- Observed the HTTPS traffic generated during the connection.

### Step 3: Save the Capture

- Stopped the packet capture after the connection was established.
- Saved the captured traffic with the filename:

```text
exp5_tls_handshake.pcapng
```

### Step 4: Display TLS Packets

- Applied the following Wireshark display filter:

```text
tls
```

- Identified the TLS packets exchanged between the client and server.

### Step 5: Examine ClientHello

- Applied the filter:

```text
tls.handshake.type == 1
```

- Opened the ClientHello packet.
- Examined the supported TLS versions, cipher suites, requested hostname, and key-share extension.
- Observed the security options proposed by the client.

### Step 6: Examine ServerHello

- Applied the filter:

```text
tls.handshake.type == 2
```

- Opened the ServerHello packet.
- Examined the selected TLS version, cipher suite, and key-share group.
- Identified the security parameters selected by the server.

### Step 7: Enable TLS Key Logging

To inspect encrypted TLS 1.3 handshake messages, browser key logging was configured.

**Windows:**

```batch
@echo off
set SSLKEYLOGFILE=%USERPROFILE%\Desktop\tls-keys.log
start firefox
```

**Linux:**

```bash
export SSLKEYLOGFILE=$HOME/Desktop/tls-keys.log
firefox
```

**macOS:**

```bash
export SSLKEYLOGFILE=$HOME/Desktop/tls-keys.log
open -a firefox
```

### Step 8: Configure TLS Decryption in Wireshark

- Opened Wireshark preferences.
- Navigated to:

```text
Edit → Preferences → Protocols → TLS
```

- Selected the generated `tls-keys.log` file under the TLS key log filename option.
- Started a new HTTPS session to capture traffic with the key logging configuration enabled.

### Step 9: Examine Protected Handshake Messages

Applied the following display filter:

```text
tls.handshake.type == 8 ||
tls.handshake.type == 11 ||
tls.handshake.type == 15 ||
tls.handshake.type == 20
```

- Examined the decrypted handshake messages, where available.
- Identified EncryptedExtensions, Certificate, CertificateVerify, and Finished messages.
- Observed how TLS 1.3 protects handshake information after ServerHello.

### Step 10: Examine Server Certificate

Navigated through the packet details:

```text
Transport Layer Security
    → Handshake Protocol: Certificate
        → Certificates
            → signedCertificate
```

Examined the following certificate fields:

- Subject and Common Name (CN).
- Certificate Issuer.
- Subject Alternative Name (SAN).
- Validity period.
- Public key information.
- Certificate chain.

## Wireshark Filters Used

| Purpose | Display Filter |
|---|---|
| HTTPS Capture Filter | `tcp port 443` |
| All TLS Packets | `tls` |
| ClientHello | `tls.handshake.type == 1` |
| ServerHello | `tls.handshake.type == 2` |
| Certificate | `tls.handshake.type == 11` |
| Protected Handshake Messages | `tls.handshake.type == 8 || tls.handshake.type == 11 || tls.handshake.type == 15 || tls.handshake.type == 20` |

## TLS Handshake Flow

```text
             CLIENT                         SERVER
                |                              |
                | -------- ClientHello ------> |
                |                              |
                | <------- ServerHello ------- |
                |                              |
                | <--- EncryptedExtensions --- |
                | <-------- Certificate ----- |
                | <----- CertificateVerify -- |
                | <---------- Finished ------ |
                |                              |
                | -------- Finished --------> |
                |                              |
                | <--- Application Data -----> |
                |                              |
                |    Secure Communication     |
```

## Results and Observations

The HTTPS traffic was captured successfully using Wireshark, and the main TLS handshake packets were identified.

| Parameter | Observation |
|---|---|
| Protocol Version | TLS 1.3 |
| Test Host | `www.wireshark.org` |
| Certificate Common Name | `wireshark.org` |
| Certificate Issuer | WE1, Google Trust Services |
| Selected Cipher Suite | To be obtained from ServerHello |
| Certificate Validity | 22 August 2026 – 20 November 2026 |

**Note:** The selected cipher suite should be filled in using the actual ServerHello packet captured during the experiment. Certificate details should also be confirmed from the captured certificate.

## Discussion

The packet analysis demonstrated how a secure connection is established between a client and a server.

The ClientHello message contains the security options supported by the client, while the ServerHello message contains the parameters selected by the server.

The server certificate helps authenticate the server's identity. In TLS 1.3, most handshake messages exchanged after ServerHello are encrypted, so a browser key log is useful for inspecting these messages in Wireshark.

This experiment helped understand how TLS protects network communication from unauthorized access and interception.

## Improvements

1. **Use a Separate HTTPS Session:** Capture a fresh HTTPS connection to avoid unnecessary packets and make analysis easier.
2. **Verify Negotiated Parameters:** Record the actual TLS version, cipher suite, and key-share group directly from ServerHello.
3. **Enable Key Logging:** Configure SSLKEYLOGFILE to inspect encrypted handshake messages when required.
4. **Improve Certificate Analysis:** Examine the certificate chain, SAN, issuer, and validity period in detail.
5. **Protect Key Log Files:** Remove temporary TLS key logs after the experiment because they contain sensitive session secrets.

## Output and Evidence

The following outputs were obtained or required for the experiment:

- ClientHello packet with expanded details.
- ServerHello packet showing selected security parameters.
- Server certificate information.
- Decrypted TLS handshake messages (if key logging is enabled).
- Wireshark packet capture file: `exp5_tls_handshake.pcapng`.
- Screenshots of the TLS handshake analysis.



