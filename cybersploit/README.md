# 🛡️ VulnHub Walkthrough: Cybersploit

> **Author**: [Preetika (@preetika-cyber)]  
> **Machine**: Cybersploit-CTF  
> **Platform**: VulnHub  
> **Difficulty**: Easy
> **URL**:https://www.vulnhub.com/entry/cybersploit-1,506/

---

## 📖 Introduction

This walkthrough documents the exploitation of the *Cybersploit-CTF* machine on VulnHub. The objective was to escalate privileges from a basic user to root and capture three hidden flags. The attack was carried out using Kali Linux as the attacker machine.

---

## 🛠️ Tools Used

- `nmap` – for port scanning
- `dirb` – for web directory brute-forcing
- `browser dev tools` – for viewing source code
- `hash-identifier` – for identifying encoded strings
- `ssh` – for remote access
- `gcc` – for compiling local privilege escalation exploit
- `cat` – for reading flag files

---
## 🔍 Scanning

### 🔎 Step 1: Network Discovery

First, we identified the IP address of the target using:

```bash
ip a
```
Then, scanned the target using Nmap:

```bash
nmap -sC -sV -oN nmap/nmap_scan.txt <target-ip>
```
Findings:

Port 22: OpenSSH
Port 80: Apache HTTP Server

![Nmap scan result](screenshots/Screenshot01.png)

