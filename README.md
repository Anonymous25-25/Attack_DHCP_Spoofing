# 🔐 DHCP Spoofing Lab — Rogue DHCP Server Detection Using Wireshark

![Status](https://img.shields.io/badge/status-completed-brightgreen)
![Category](https://img.shields.io/badge/category-Network%20Security-blue)
![Tool](https://img.shields.io/badge/tool-Wireshark-1679A7)
![Platform](https://img.shields.io/badge/platform-VirtualBox-183A61)

## 📌 Overview

This lab demonstrates how a **Rogue DHCP Server** can be simulated on an isolated network, how it competes with a legitimate DHCP server for client leases, and how to **detect** this behavior using **Wireshark**.

The goal is purely educational: understand DHCP internals, observe a real Rogue DHCP scenario in a controlled lab, and document detection and mitigation strategies (DHCP Snooping).

> ⚠️ **Disclaimer:** This lab was performed entirely inside an isolated VirtualBox "Internal Network." No production network, third-party system, or real infrastructure was targeted. All techniques are documented for educational and defensive (blue-team) purposes only.

---

## 🗂️ Lab Environment

| Machine | Role | IP Address |
|---|---|---|
| Windows Server | Legitimate DHCP Server | `192.168.11.1` |
| Kali Linux | Rogue DHCP Simulation | `192.168.11.201` |
| Windows 10 | DHCP Client | `192.168.11.200` → `192.168.11.228` |

- Network: `192.168.11.0/24` (isolated VirtualBox Internal Network — `DHCP_LAB`)
- Legitimate DHCP scope: `192.168.11.200 – 192.168.11.250`
- Rogue DHCP scope (dnsmasq): `192.168.11.220 – 192.168.11.230`

---

## 🧠 What This Lab Covers

1. **DHCP Protocol Fundamentals** — the DORA process (Discover, Offer, Request, ACK) and DHCP Option 54 (Server Identifier).
2. **Legitimate DHCP Server Setup** — Windows Server role installation, scope creation and activation.
3. **Rogue DHCP Simulation** — `dnsmasq` on Kali Linux configured as a second, unauthorized DHCP server.
4. **Traffic Analysis & Detection** — identifying competing DHCP Offers/ACKs/NAKs in Wireshark, flagging the unexpected server identifier.
5. **Security Impact** — rogue gateway/DNS risk, network disruption, potential traffic interception (not verified in this lab).
6. **Mitigation** — DHCP Snooping deployment guidance (trusted/untrusted ports).

---

## 🛠️ Tools Used

- **VirtualBox** — isolated lab network
- **Windows Server 2022** — legitimate DHCP/DNS role
- **Kali Linux** — `dnsmasq`, rogue DHCP simulation
- **Wireshark** — packet capture & protocol analysis

---

## 🔎 Key Wireshark Filters

```text
# DHCP Offer messages
dhcp.option.dhcp == 2

# DHCP Request / ACK / NAK messages
dhcp.option.dhcp == 3 || dhcp.option.dhcp == 5 || dhcp.option.dhcp == 6
```

---

## 📄 Full Report

The complete write-up — methodology, step-by-step screenshots, packet analysis, security impact, limitations, and mitigation — is available here:

📥 [`DHCP_Spoofing_Lab_Report_Youness_Boussedari.pdf`](./DHCP_Spoofing_Lab_Report_Youness_Boussedari.pdf)

---

## 🛡️ Mitigation Summary

The main defense against Rogue DHCP Servers is **DHCP Snooping** on supported switches:

1. Enable DHCP Snooping on the relevant VLANs.
2. Mark ports connected to authorized DHCP servers as **trusted**.
3. Leave client-facing ports **untrusted**.
4. Monitor DHCP logs and investigate unexpected DHCP Server Identifiers.

---

## 👤 Author

**Youness Boussedari**
Technicien en Cybersécurité — Test d'Intrusion, SOC Analyste
📍 Sidi Kacem, Morocco
🔗 [LinkedIn](https://www.linkedin.com/in/youness-boussedari) · [Portfolio](https://blacksidesecurity.site) · [GitHub](https://github.com/Anonymous25-25)

---

## 🏷️ Tags

`#CyberSecurity` `#DHCPSpoofing` `#NetworkSecurity` `#Wireshark` `#PenetrationTesting` `#SOC` `#KaliLinux` `#RedTeam` `#BlueTeam`
