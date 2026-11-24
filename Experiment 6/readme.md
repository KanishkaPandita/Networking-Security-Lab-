# Lab 06 — Secure Remote Administration Using OpenSSH

## 1. Aim

To configure secure remote administration using OpenSSH by setting up an SSH server on Kali Linux, verifying the server's host key fingerprint from Windows, and implementing Ed25519 public-key authentication for passwordless remote login.

## 2. Overview

Secure Shell (SSH) is a network protocol used to securely access and manage remote systems over an unsecured network. It provides encrypted communication between a client and a server, protecting transmitted data from unauthorized access.

In this experiment, OpenSSH Server was configured on a Kali Linux virtual machine, while Windows was used as the SSH client. The server identity was verified using its SHA-256 host key fingerprint.

An Ed25519 key pair was then generated on Windows, and the public key was added to the Kali Linux server's `authorized_keys` file. Finally, key-based authentication was tested without allowing password authentication for the test connection.

## 3. Tools and Technologies

- **Kali Linux:** Remote SSH server.
- **Windows 10/11:** SSH client workstation.
- **OpenSSH:** Secure remote administration software.
- **Ed25519:** Modern elliptic-curve public-key algorithm.
- **TCP Port 22:** Default SSH communication port.
- **PowerShell / Command Prompt:** For executing SSH commands.
- **VMware:** Virtualization environment for the lab.

## 4. System Configuration

| Component | Configuration |
|---|---|
| Client | Windows 10/11 |
| Server | Kali Linux VM |
| Protocol | SSH |
| Port | TCP 22 |
| Authentication | Ed25519 Public-Key Authentication |
| Key Type | `ssh-ed25519` |
| Network | Local Virtual Lab Network |

## 5. Methodology

### Stage A: Verify Client and Server Environment

**Step 1: Check OpenSSH Client Version**

- Opened Windows PowerShell or Command Prompt.
- Checked whether the OpenSSH client was installed.

Command:

```bash
ssh -V
```

**Step 2: Install and Enable OpenSSH Server on Kali Linux**

- Updated the Kali Linux package list.
- Installed the OpenSSH server package.
- Enabled and started the SSH service.

Commands:

```bash
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
```

**Step 3: Verify SSH Service and Port**

- Checked whether the SSH service was running.
- Verified that the server was listening on TCP port 22.

Commands:

```bash
systemctl status ssh --no-pager
sudo ss -tlnp | grep ':22'
```

**Step 4: Identify Server Details**

Retrieved the current username, hostname, and IP address of the Kali Linux machine.

Commands:

```bash
whoami
hostname
hostname -I
```

### Stage B: Host Key Verification and Password Authentication

**Step 1: Retrieve Server Host Key Fingerprint**

Generated the SHA-256 fingerprint of the Ed25519 host public key on Kali Linux.

Command:

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub
```

**Step 2: Connect from Windows to Kali Linux**

Used the SSH client to establish a remote connection.

Command:

```bash
ssh <username>@<server_ip>
```

**Step 3: Verify Server Identity**

- Compared the SHA-256 fingerprint displayed by the Windows SSH client with the fingerprint obtained directly from Kali Linux.
- Confirmed that both fingerprints matched.
- Accepted the host key by entering `yes`.
- Entered the Kali Linux account password.

The verified server key was stored in the Windows `known_hosts` file.

**Step 4: Verify Remote Shell**

Executed basic commands to confirm successful remote access.

Commands:

```bash
whoami
hostname
pwd
exit
```

### Stage C: Generate and Install Ed25519 Keys

**Step 1: Generate Ed25519 Key Pair on Windows**

Generated a public-private key pair using the Ed25519 algorithm.

Command:

```bash
ssh-keygen -t ed25519 -C "student@lab"
```

The generated files were:

- `id_ed25519` — Private key.
- `id_ed25519.pub` — Public key.

**Step 2: Verify Public Key Fingerprint**

Generated the fingerprint of the public key.

Command:

```bash
ssh-keygen -lf $HOME\.ssh\id_ed25519.pub
```

**Step 3: Install Public Key on Kali Linux**

Transferred the public key from Windows to the Kali Linux account and added it to the `authorized_keys` file.

Command:

```powershell
type $HOME\.ssh\id_ed25519.pub | ssh <username>@<server_ip> "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

**Step 4: Set Secure Permissions**

Applied appropriate permissions to protect SSH authentication files.

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

**Security Note:** Only the public key was transferred to the server. The private key remained securely stored on the Windows client.

### Stage D: Validate Key-Based Authentication

**Step 1: Test Public-Key-Only Login**

Attempted a remote connection with password authentication disabled for this SSH connection.

Command:

```bash
ssh -o PreferredAuthentications=publickey -o PasswordAuthentication=no <username>@<server_ip>
```

**Step 2: Verify Remote Access**

After successful authentication, executed the following commands:

```bash
whoami
hostname
exit
```

This confirmed that the SSH server accepted the Ed25519 public key without requiring an account password.

## 6. Common Errors and Troubleshooting

| Error | Possible Reason | Solution |
|---|---|---|
| Connection refused | SSH service is stopped | Start SSH using `sudo systemctl start ssh` |
| Connection timeout | Incorrect IP or network configuration | Verify the server IP and VM network adapter |
| Permission denied | Public key not authorized | Check `authorized_keys` and file permissions |
| Host key verification failed | Server host key has changed | Verify the new fingerprint before updating `known_hosts` |
| SSH service not found | OpenSSH server not installed | Install using `sudo apt install openssh-server` |

**Additional Debugging Commands**

Check SSH connection details:

```bash
ssh -vv <username>@<server_ip>
```

Monitor SSH authentication logs:

```bash
sudo journalctl -fu ssh.service
```

## 7. Results and Observations

The experiment successfully demonstrated secure remote administration using OpenSSH.

| Parameter | Observation |
|---|---|
| SSH Client | Windows OpenSSH |
| SSH Server | Kali Linux |
| Default Port | 22 |
| Host Key Algorithm | Ed25519 |
| Host Fingerprint | SHA-256 |
| User Authentication | Ed25519 Public-Key Authentication |
| Remote Connection | Successful |
| Password Authentication | Disabled for the key-only test connection |

### Key Observations

1. Successfully installed and started the OpenSSH server on Kali Linux.
2. Verified that the SSH daemon was listening on TCP port 22.
3. Retrieved and verified the server's SHA-256 host key fingerprint.
4. Established a remote SSH session using password authentication.
5. Generated an Ed25519 public-private key pair on Windows.
6. Successfully installed the public key in the Kali Linux `authorized_keys` file.
7. Established a remote session using public-key-only authentication.

## 8. Security Improvements

1. **Disable Password Authentication:** After verifying key-based access and ensuring recovery access, disable password authentication on the server to reduce password-based attacks.
2. **Use Strong Key Protection:** Protect the private key with a passphrase and restrict access to the key file.
3. **Verify Host Fingerprints:** Always compare server fingerprints through a trusted channel before accepting a new host key.
4. **Restrict SSH Access:** Allow SSH connections only from authorized users and trusted network addresses.
5. **Monitor Authentication Logs:** Regularly check SSH logs for repeated failed login attempts and suspicious access.
6. **Keep OpenSSH Updated:** Install security updates to protect against known vulnerabilities.

## 9. Lab Deliverables

The following activities were completed during the experiment:

-  Verified OpenSSH client version on Windows.
-  Installed and enabled OpenSSH server on Kali Linux.
-  Checked SSH daemon status and TCP port 22.
-  Retrieved the server's SHA-256 host fingerprint.
-  Verified the server identity before accepting the connection.
-  Established a remote shell using password authentication.
-  Generated an Ed25519 key pair on Windows.
-  Verified the public key fingerprint.
-  Added the public key to the server's `authorized_keys` file.
-  Successfully tested public-key-only authentication.

