# Experiment 10: Perimeter Firewall & Snort IDS Deployment with pfSense

## COM-511: Network Security Lab

**Project Title:** Perimeter Firewall and Intrusion Detection System Deployment using pfSense and Snort

**Technologies:** pfSense 2.8.1, Snort IDS, Kali Linux, Metasploitable 2, Nmap, Hydra, Wireshark

---

## 1. Project Overview

This experiment demonstrates the implementation of a layered network security architecture using **pfSense as a perimeter firewall** and **Snort as a signature-based Intrusion Detection System (IDS)**.

An isolated virtual network was created to monitor and control traffic between Kali Linux and Metasploitable 2. Firewall rules were configured to allow permitted services and block restricted connections.

Different security testing activities, including port scanning, FTP brute-force simulation, and ICMP traffic generation, were performed to evaluate Snort's detection capabilities.

## 2. Objectives

- Deploy pfSense as a perimeter firewall.
- Configure WAN and LAN network interfaces.
- Implement stateful firewall rules to control network traffic.
- Integrate Snort IDS with pfSense.
- Detect suspicious activities using signature-based detection.
- Simulate controlled attacks using Kali Linux.
- Analyze firewall logs and Snort alerts.

## 3. Lab Environment

| Component | Configuration |
|---|---|
| Firewall | pfSense 2.8.1 |
| IDS | Snort |
| Attacker | Kali Linux |
| Target | Metasploitable 2 |
| Network | Isolated Virtual LAN |
| WAN IP | 10.0.2.15 |
| LAN Gateway | 192.168.1.1 |
| Kali IP | 192.168.1.10 |
| Target IP | 192.168.1.20 |

## 4. Network Architecture

```text
+------------------------+
|     Attacker Node      |
|      Kali Linux        |
|                        |
| IP: 192.168.1.10       |
+-----------+------------+
            |
            | LAN Traffic
            |
+-----------v----------------------+
|       pfSense Firewall           |
|       + Snort IDS                |
|                                  |
|       WAN: 10.0.2.15             |
|       LAN: 192.168.1.1           |
|                                  |
|   Firewall Filtering             |
|   Intrusion Detection            |
+-----------+----------------------+
            |
            | Protected LAN
            |
+-----------v------------+
|    Target Server       |
|    Metasploitable 2    |
|                        |
|    IP: 192.168.1.20    |
+------------------------+
```

## 5. Technologies Used

- **pfSense:** Stateful firewall used to filter and control network traffic.
- **Snort:** Signature-based IDS used to detect suspicious network activities.
- **Kali Linux:** Security testing workstation used to generate test traffic.
- **Metasploitable 2:** Intentionally vulnerable machine used as the target.
- **Nmap:** Network scanning and port discovery tool.
- **Hydra:** Password auditing tool used for controlled FTP authentication testing.
- **Wireshark:** Network packet capture and analysis tool.

## 6. Implementation Steps

### Step 1: Configure pfSense Network Interfaces

- Installed pfSense in a virtual machine.
- Assigned `em0` as the WAN interface.
- Assigned `em1` as the LAN interface.
- Configured the LAN IP address as `192.168.1.1/24`.
- Enabled the LAN DHCP server.

Web interface:

```text
https://192.168.1.1
```

### Step 2: Configure Kali Linux and Metasploitable 2

Configured both virtual machines on the internal LAN network.

**Kali Linux:**

```bash
sudo ip addr add 192.168.1.10/24 dev eth0
sudo ip route replace default via 192.168.1.1
```

**Metasploitable 2:**

```bash
sudo ifconfig eth0 192.168.1.20 netmask 255.255.255.0
sudo route add default gw 192.168.1.1
```

Verified connectivity using:

```bash
ping 192.168.1.1
ping 192.168.1.20
```

### Step 3: Configure Firewall Rules

Navigated to:

`Firewall → Rules → LAN`

Configured rules to allow HTTP, HTTPS, and DNS traffic while blocking other restricted traffic.

| Priority | Action | Protocol | Destination Port |
|---|---|---|---|
| 1 | Pass | Any | 80, 443 to LAN Address |
| 2 | Pass | TCP | 80 |
| 3 | Pass | TCP | 443 |
| 4 | Pass | UDP | 53 |
| 5 | Block | Any | Any |

**Note:** Firewall rules are evaluated from top to bottom. The anti-lockout rule protects access to the pfSense web interface. The final block rule restricts traffic that does not match an earlier pass rule.

### Step 4: Install and Configure Snort

1. Opened the pfSense web interface.
2. Navigated to `System → Package Manager`.
3. Installed the Snort package.
4. Opened `Services → Snort`.
5. Added a new Snort instance on the LAN interface.
6. Enabled the available community and Emerging Threats rulesets.
7. Enabled the detection engine and started Snort.

### Step 5: Firewall Filtering Validation

From Kali Linux, tested permitted and restricted services.

**Test HTTP connectivity:**

```bash
curl -I http://192.168.1.20
```

Expected result: HTTP response from the target when the HTTP pass rule matches.

**Test restricted Telnet traffic:**

```bash
telnet 192.168.1.20 23
```

Expected result: Connection blocked or timed out when the block rule matches.

Checked blocked connection attempts under:

`Status → System Logs → Firewall`

### Step 6: Intrusion Detection Testing

Performed controlled security testing from Kali Linux against the isolated target.

**1. TCP SYN / Port Scanning**

```bash
nmap -sS -p 1-1000 192.168.1.20
```

Purpose: Identify open ports and generate traffic that can match network scanning signatures.

**2. FTP Authentication Testing**

```bash
hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt ftp://192.168.1.20
```

Purpose: Generate controlled FTP authentication attempts to test detection of repeated login activity.

**3. ICMP Traffic Testing**

```bash
ping -c 50 -i 0.2 192.168.1.20
```

Purpose: Generate a short burst of ICMP packets to observe traffic monitoring and applicable IDS alerts.

**Important:** These tests should only be performed inside the authorized isolated lab environment.

## 7. Verification and Observations

### Firewall Verification

- HTTP traffic was tested against the configured allow rule.
- Restricted Telnet traffic was tested against the blocking policy.
- Firewall logs were inspected for blocked connection attempts.
- Source and destination IP addresses were checked in the logs.

### Snort IDS Verification

Navigated to:

`Services → Snort → Alerts`

Observed alerts corresponding to suspicious traffic when matching signatures were enabled.

| Test Activity | Expected Detection | Observation |
|---|---|---|
| Nmap Port Scan | Network scanning signatures | Scan-related alerts |
| FTP Login Attempts | FTP authentication-related signatures, if enabled | Repeated login activity |
| ICMP Traffic Burst | Applicable ICMP signatures, if enabled | ICMP traffic alerts |
| Telnet Connection | Firewall block event | Restricted connection logged |

**Note:** Exact Snort alert names and SIDs depend on the installed ruleset and its configuration. A test activity does not guarantee a matching alert.

## 8. Key Security Observations

1. pfSense successfully provided centralized network traffic filtering.
2. Firewall rules controlled access to selected network services.
3. Restricted connection attempts could be examined through firewall logs.
4. Snort monitored LAN traffic and generated alerts for matching signatures.
5. Nmap scanning helped evaluate network reconnaissance detection.
6. FTP authentication testing demonstrated how repeated login attempts can be monitored.
7. ICMP traffic generation helped evaluate network traffic monitoring.

## 9. Future Improvements

1. **Enable Intrusion Prevention:** Configure Snort in IPS mode where supported and carefully test blocking rules.
2. **Update Snort Rules:** Regularly update detection signatures to identify newly discovered threats.
3. **Centralized Log Monitoring:** Integrate pfSense and Snort logs with an ELK or Splunk SIEM platform.
4. **Advanced Alerting:** Configure notifications for repeated authentication failures and suspicious traffic.
5. **Network Segmentation:** Separate servers, workstations, and management systems into different VLANs.
6. **Traffic Baseline Monitoring:** Establish normal network traffic patterns to identify unusual activities.
7. **Regular Security Audits:** Review firewall policies and IDS alerts to improve network protection.

## 10. Result

Successfully deployed pfSense as a perimeter firewall and integrated Snort as an Intrusion Detection System. Firewall rules were configured to control network traffic, and controlled testing activities were performed using Kali Linux against Metasploitable 2. Firewall logs and Snort alerts were examined to evaluate network filtering and intrusion detection capabilities.

## 11. Conclusion

This experiment provided practical knowledge of perimeter firewall deployment, stateful packet filtering, and signature-based intrusion detection. The integration of pfSense and Snort demonstrated how firewall policies and IDS monitoring work together to improve network security. The lab also provided hands-on experience in analyzing suspicious traffic and identifying potential network threats.

---

**Experiment:** 10  
**Subject:** COM-511 Network Security Lab  
**Platform:** VMware / Virtual Lab  
**Tools:** pfSense | Snort | Kali Linux | Metasploitable 2 | Nmap | Hydra

**Disclaimer:** All security testing was performed in an isolated and authorized virtual lab environment. Metasploitable 2 is intentionally vulnerable and should never be exposed to public networks.
