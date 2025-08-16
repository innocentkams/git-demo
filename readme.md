
---

# Simulated MITM Attacks in a Multi-VM Lab

## 📜 Overview

This repository contains a **complete training framework** for simulating and analyzing **Man-in-the-Middle (MITM) attacks** in a controlled multi-VM environment.
The content is designed for cybersecurity training, penetration testing labs, and academic research.

You will learn:

* **Theory** behind ARP spoofing, DHCP spoofing, and TELNET MITM.
* **Hands-on execution** using tools like **Ettercap**, **Cain & Abel**, and **Wireshark**.
* **Forensic packet analysis** techniques.
* **Defense & mitigation strategies** against MITM attacks.

⚠️ **Ethical Notice:**
All demonstrations **must be performed only in an isolated lab** with explicit permission.
Misuse of these techniques is **illegal** and punishable under cybercrime laws.

---

## 📚 Table of Contents

1. [Lab Environment Setup](#-lab-environment-setup)
2. [Lab 1: ARP Spoofing MITM using Ettercap](#-lab-1-arp-spoofing-mitm-using-ettercap)
3. [Lab 2: DHCP Spoofing using Ettercap](#-lab-2-dhcp-spoofing-using-ettercap)
4. [Lab 3: TELNET MITM using Cain & Abel](#-lab-3-telnet-mitm-using-cain--abel)
5. [Lab 4: Wireshark Configuration & Forensic Analysis](#-lab-4-wireshark-configuration--forensic-analysis)
6. [Defense & Mitigation Strategies](#-defense--mitigation-strategies)
7. [Glossary](#-glossary)
8. [References](#-references)

---

## 🖥 Lab Environment Setup

All labs use **virtual machines** in an **isolated network**.

**Recommended VM Setup:**

* **Attacker:** Kali Linux VM (Ettercap & Wireshark installed)
* **Victims:** Windows 10 & Windows 11 VMs
* **Target:** Metasploitable VM (for TELNET)
* **Networking Mode:** Internal/NAT Network (no internet access unless explicitly required)

**Network Configuration Checks:**

```bash
# On Kali, enable IP forwarding:
echo 1 > /proc/sys/net/ipv4/ip_forward

# Verify IP forwarding:
cat /proc/sys/net/ipv4/ip_forward  # Should output '1'
```

---

## 🧪 Lab 1: ARP Spoofing MITM using Ettercap

### Objective

Intercept and analyze unencrypted network traffic between two hosts using **ARP spoofing**.

### Key Concepts

* **ARP Vulnerability:** Stateless protocol, trusts any reply.
* **Spoofing:** Forge ARP replies to redirect traffic.
* **IP Forwarding:** Prevents connection loss during MITM.

### Steps

1. Enable IP forwarding on Kali.
2. Start Ettercap in GUI mode:

   ```bash
   sudo ettercap -G
   ```
3. Select interface → Host scan → Identify Victim & Gateway → Assign as Target 1 & Target 2.
4. Start ARP poisoning with **Sniff Remote Connections** enabled.
5. Capture traffic with Wireshark.
6. Generate victim activity (HTTP/TELNET) and observe captured credentials.

### Expected Results

* ARP table on victim shows attacker’s MAC for gateway IP.
* Wireshark captures plaintext credentials over HTTP/TELNET.

---

## 🧪 Lab 2: DHCP Spoofing using Ettercap

### Objective

Use a **rogue DHCP server** to redirect victim traffic via attacker.

### Key Concepts

* **DHCP DORA Process:** Discover, Offer, Request, ACK.
* **Vulnerability:** No authentication for DHCP servers.
* **Attack Goal:** Provide attacker’s IP as default gateway.

### Steps

1. Disable DHCP in VM network settings.
2. Assign static IP to Kali (e.g., 192.168.56.1).
3. Start Ettercap GUI → Enable `dhcp_spoof` plugin.
4. Restart victim’s network interface to trigger DHCP.
5. Verify victim’s IP, gateway, and DNS point to attacker.
6. Capture and analyze redirected traffic in Wireshark.

---

## 🧪 Lab 3: TELNET MITM using Cain & Abel

### Objective

Sniff plaintext TELNET credentials using **Cain & Abel**.

### Key Concepts

* **TELNET Weakness:** Plaintext transmission.
* **Attack Flow:** ARP poisoning → Credential capture.

### Steps

1. Install Cain & Abel on Windows attacker VM.
2. Enable Sniffer → Perform ARP scan → Identify Gateway & Victim.
3. Assign targets in APR tab → Start poisoning.
4. Initiate TELNET session from victim to Metasploitable.
5. View captured credentials in Cain’s "Passwords" tab.

---

## 🧪 Lab 4: Wireshark Configuration & Forensic Analysis

### Objective

Learn **capture filters**, **display filters**, and **analysis tools** in Wireshark.

### Key Skills

* `arp`, `bootp`, `tcp.port == 23`, `http` filters.
* **Follow TCP Stream** to reconstruct sessions.
* **Export Objects → HTTP** to extract files.
* **Statistics → Conversations** to detect anomalies.

---

## 🛡 Defense & Mitigation Strategies

| Threat           | Defense Technique         | Description                        |
| ---------------- | ------------------------- | ---------------------------------- |
| ARP Spoofing     | Static ARP Entries        | Prevents unauthorized MAC changes. |
| DHCP Spoofing    | DHCP Snooping on Switches | Blocks rogue DHCP offers.          |
| MITM in LAN      | VLAN Segmentation         | Limits broadcast domain.           |
| General MITM     | IDS/IPS Deployment        | Detects abnormal ARP/DHCP traffic. |
| Passive Sniffing | HTTPS / SSH Encryption    | Makes intercepted data unreadable. |

---

## 📖 Glossary

* **MITM:** Man-in-the-Middle.
* **ARP:** Address Resolution Protocol.
* **DHCP:** Dynamic Host Configuration Protocol.
* **DORA:** Discover, Offer, Request, Acknowledge.
* **TELNET:** Unencrypted remote terminal protocol.
* **Ettercap:** MITM attack toolkit.
* **Cain & Abel:** Windows password recovery/sniffing tool.

---

## 📚 References

* Ettercap Project: [https://www.ettercap-project.org/](https://www.ettercap-project.org/)
* Wireshark Documentation: [https://www.wireshark.org/docs/](https://www.wireshark.org/docs/)
* Cain & Abel Archive: [https://www.oxid.it/cain.html](https://www.oxid.it/cain.html)

---
