
# Experiment 9: Site-to-Site IPsec VPN in Tunnel Mode using strongSwan

## COM-511: Network Security Lab

**Project Title:** Site-to-Site IPsec VPN in Tunnel Mode using strongSwan (`swanctl`)

**Technology:** Kali Linux, Ubuntu Linux, strongSwan, IKEv2, ESP, Wireshark, tcpdump

---

## 1. Project Overview

This experiment demonstrates the configuration and implementation of a Site-to-Site IPsec VPN in Tunnel Mode using strongSwan on two Linux virtual machines.

IKEv2 is used for authentication and Security Association (SA) establishment, while ESP provides encryption, integrity, and protection for network traffic.

The experiment verifies secure communication between two private loopback addresses over an untrusted network and analyzes encrypted packets using Wireshark.

## 2. Objectives

- Configure a Site-to-Site IPsec VPN using strongSwan.
- Understand IKEv2 negotiation and Security Associations.
- Implement ESP encryption in Tunnel Mode.
- Establish secure communication between two Linux gateways.
- Capture and analyze encrypted VPN traffic using Wireshark.
- Verify IPsec policies and states using Linux XFRM commands.

## 3. Lab Environment

| Component | Gateway A | Gateway B |
|---|---|---|
| Operating System | Kali Linux | Ubuntu Linux |
| Role | Initiator | Responder |
| Outer Interface | eth1 | ens34 |
| Outer IP | 192.168.159.140 | 192.168.159.145 |
| Inner IP | 10.10.1.1 | 10.20.1.1 |
| IKE Identity | gw-a | gw-b |
| VPN Protocol | IKEv2 | IKEv2 |

### Network Topology

```text
+----------------------+          +----------------------+
|    Gateway A         |          |     Gateway B        |
|    Kali Linux        |          |     Ubuntu Linux     |
|                      |          |                      |
| Inner IP:            |          | Inner IP:            |
| 10.10.1.1            |          | 10.20.1.1            |
|                      |          |                      |
| Outer IP:            |          | Outer IP:            |
| 192.168.159.140      |          | 192.168.159.145      |
+----------+-----------+          +-----------+----------+
           |                                  |
           +========== IPsec VPN =============+
                      IKEv2 + ESP
                    Tunnel Mode
```

## 4. Technologies Used

- **Kali Linux:** Used as the initiating VPN gateway.
- **Ubuntu Linux:** Used as the responding VPN gateway.
- **strongSwan:** Open-source IPsec VPN implementation.
- **IKEv2:** Establishes secure connections and negotiates keys.
- **ESP:** Encrypts and authenticates protected network traffic.
- **swanctl:** Manages VPN connections and Security Associations.
- **Wireshark:** Captures and analyzes VPN packets.
- **tcpdump:** Captures network traffic from the outer interface.
- **Linux XFRM:** Manages IPsec states and policies in the Linux kernel.

## 5. Implementation Steps

### Step 1: Install Required Packages

Run the following commands on both Linux gateways:

```bash
sudo apt update
sudo apt install -y charon-systemd strongswan-swanctl tcpdump
```

### Step 2: Configure Inner Loopback IP Addresses

On Gateway A:

```bash
sudo ip address add 10.10.1.1/32 dev lo
```

On Gateway B:

```bash
sudo ip address add 10.20.1.1/32 dev lo
```

### Step 3: Configure strongSwan

Edit the configuration file:

```bash
sudo nano /etc/swanctl/swanctl.conf
```

Configure the VPN connection with:

- IKEv2 protocol
- AES-256-GCM encryption
- SHA-256 PRF
- MODP-3072 Diffie-Hellman group
- Pre-Shared Key authentication
- ESP encryption in Tunnel Mode
- Local and remote traffic selectors

Use the corresponding local and remote IP addresses and identities on each gateway.

### Step 4: Load VPN Configuration

```bash
sudo chmod 600 /etc/swanctl/swanctl.conf
sudo swanctl --load-all
sudo swanctl --list-conns
```

### Step 5: Start VPN Connection

On Gateway A:

```bash
sudo swanctl --initiate --child lab-net
```

### Step 6: Verify Security Associations

```bash
sudo swanctl --list-sas
```

Check whether the IKE SA is established and the CHILD SA is installed.

### Step 7: Verify IPsec States and Policies

```bash
sudo ip xfrm state
sudo ip xfrm policy
```

### Step 8: Test Secure Communication

From Gateway A:

```bash
ping -I 10.10.1.1 -c 4 10.20.1.1
```

Successful replies indicate that protected traffic is passing through the VPN tunnel.

### Step 9: Capture and Analyze Traffic

Start packet capture on Gateway A:

```bash
sudo tcpdump -ni eth1 -w exp9-ipsec.pcap 'udp port 500 or udp port 4500 or proto 50 or proto 51'
```

Open the captured file in Wireshark and apply the display filter:

```text
isakmp || esp || ah
```

Observe IKEv2 negotiation packets and encrypted ESP traffic.

## 6. Security Parameters

| Parameter | Configuration |
|---|---|
| VPN Protocol | IKEv2 |
| Authentication | Pre-Shared Key |
| Encryption | AES-256-GCM |
| PRF | SHA-256 |
| Diffie-Hellman | MODP-3072 |
| Data Protection | ESP |
| VPN Mode | Tunnel Mode |
| Traffic Selectors | 10.10.1.1/32 ↔ 10.20.1.1/32 |

## 7. Verification and Observations

The following results were observed during the experiment:

1. IKEv2 Security Association was successfully established.
2. CHILD SA was installed in Tunnel Mode.
3. ESP packets were captured between the two outer IP addresses.
4. Linux XFRM states and policies were successfully installed.
5. ICMP communication was successful with 0% packet loss.
6. Wireshark showed encrypted ESP packets instead of readable inner traffic.
7. No AH packets were observed because ESP was used for encryption and integrity protection.

## 8. Wireshark Analysis

The packet capture demonstrated three important stages:

- **IKE_SA_INIT:** Negotiation of cryptographic algorithms and Diffie-Hellman key exchange.
- **IKE_AUTH:** Authentication of both gateways and CHILD SA negotiation.
- **ESP Traffic:** Encrypted data transmission between the VPN gateways.

The outer IP addresses and ESP headers remained visible, while the protected inner IP addresses and ICMP payload were encrypted.

## 9. Result

The Site-to-Site IPsec VPN was successfully configured between Kali Linux and Ubuntu Linux using strongSwan and IKEv2. The ESP protocol provided encryption and integrity protection in Tunnel Mode. Successful ping communication and Wireshark packet analysis verified the secure transmission of network traffic.

## 10. Future Improvements

1. **Certificate-Based Authentication:** Replace PSK authentication with digital certificates for stronger identity verification.
2. **Enable Perfect Forward Secrecy (PFS):** Configure PFS for additional protection of session keys.
3. **Strong Encryption:** Continue using modern encryption algorithms such as AES-256-GCM.
4. **Automatic Key Rotation:** Configure regular rekeying of IKE and CHILD Security Associations.
5. **VPN Monitoring:** Integrate monitoring tools to detect suspicious VPN activity.
6. **High Availability:** Configure backup VPN gateways to maintain connectivity during failures.

## 11. Cleanup

Terminate the VPN connection:

```bash
sudo swanctl --terminate --child lab-net
```

Remove the inner loopback address:

```bash
sudo ip address del 10.10.1.1/32 dev lo
```

Perform the equivalent cleanup on Gateway B.

## 12. Conclusion

This experiment provided practical knowledge of Site-to-Site IPsec VPN configuration, IKEv2 negotiation, Security Associations, ESP encryption, and Linux XFRM policies. Packet analysis using Wireshark demonstrated how IPsec protects network communication over an untrusted network.

---

**Experiment:** 09  
**Subject:** COM-511 Network Security Lab  
**Tools:** Kali Linux | Ubuntu | strongSwan | Wireshark | tcpdump

**Note:** The Pre-Shared Key has been redacted for security purposes. Never upload actual VPN credentials or private keys to a public GitHub repository.
