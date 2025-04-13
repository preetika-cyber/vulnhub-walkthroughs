# 🛡️ VulnHub Walkthrough: Deathnote

> **Author**: [Preetika (@preetika-cyber)]  
> **Machine**: Deathnote: 1  
> **Platform**: VulnHub  
> **Difficulty**: Easy-Medium
> **URL**:https://www.vulnhub.com/entry/deathnote-1,739/

---

## 🧠 Introduction

This penetration test was conducted on a vulnerable machine named **Death Note**, hosted on the VulnHub platform. The goal was to identify and exploit vulnerabilities to gain root access, simulating a real-world attack scenario.

---

### ⚠️ Disclaimer
I am using VMware Workstation to run the downloaded vulnerable machine (Deathnote) and Kali Linux as the attacking machine for this Capture The Flag (CTF) challenge.
🛡️ All techniques demonstrated in this walkthrough are intended solely for educational and ethical purposes. This is part of my effort to practice and enhance cybersecurity skills in a controlled, legal environment.
❗Please do not attempt these techniques on any system without explicit permission.

---

## 🛠️ Methodology

Tools used:
- Netdiscover
- Nmap
- /etc/hosts
- WPScan
- Hydra
- Wget
- CyberChef

---

## 🔍 Scanning & Enumeration

### Step 1: Checking MAC Address
- Set up the lab environment and check the MAC address of the target VM.  
![](Screenshots/1_mac_address.png)

### Step 2: Discovering IP Address
- Use `Netdiscover` to find the target's IP in the network.  
![](Screenshots/2_netdiscover.png)
![](Screenshots/3_ip_address.png)

- Verify connectivity with `ping`.  
![](Screenshots/4_ping.png)

### Step 3: Nmap Port Scan
```bash
nmap -A <target-ip>
```
Discover open ports and services.
![](Screenshots/5_nmap.png)

### Step 4: HTTP Access
Visited port 80, but domain didn't resolve.
![](Screenshots/6_port80.png)

Modified /etc/hosts:
```bash
<target-ip> deathnote.vuln
```
![](Screenshots/7_etc_hosts.png)
![](Screenshots/8_etc_hosts.png)
![](Screenshots/9_etc_hosts.png)
Added the target’s IP and hostname to the /etc/hosts file for easier access.

### Step 5: Website Exploration
Found a "Hint" section on the website.
![](Screenshots/10_port80.png)
![](Screenshots/11_http.png)
![](Screenshots/12_hint.png)

Hint tells us to find a file notes.txt
![](Screenshots/13_hint.png)

### Step 6: WPScan Usage

```bash
wpscan --url http://deathnote.vuln --enumerate u
```
Enumerated WordPress setup.
![](Screenshots/14_wpscan.png)
![](Screenshots/15_directory.png)

Discovered notes.txt and user.txt.
![](Screenshots/16_directory.png)
![](Screenshots/17_notes_txt.png)

## 💥 Exploitation

### Step 7: Hydra Brute-Force
Downloaded notes.txt and user.txt via wget.
![](Screenshots/18_wget.png)
![](Screenshots/19_wget.png)

Using hydra tool for ssh access
![](Screenshots/20_hydra.png)

```bash
hydra -L user.txt -P notes.txt deathnote.vuln ssh
```
Successful login credentials found.
![](Screenshots/21_ssh.png)

### Step 8: SSH Access

```bash
ssh <user>@deathnote.vuln
```
Found user.txt with Brainfuck encoded message.
![](Screenshots/22_ssh.png)

Decoded Brainfuck and found a message.
![](Screenshots/23_user_txt.png)

No permission to read kira.txt.
![](Screenshots/24_kira_txt.png)

Explored /opt/L/ and found:

kira-case

fake-notebook-rule

case.wav
![](Screenshots/25_kira-case.png)
![](Screenshots/26_case_wav.png)

Hint pointed to CyberChef. Extracted hash from .wav file.
![](Screenshots/27_case_wav.png)
![](Screenshots/28_hash_type.png)

Identified as Base64. Decoded hash:
passwd: kiraisevil

![](Screenshots/29_hash_decode.png)

## ⬆️ Privilege Escalation

### Step 9: Switching User to Kira

```bash
su kira
```
Used password kiraisevil and switched user to kira.
Gained permission to read kira.txt.
![](Screenshots/30_kira_txt.png)

Decoded another message inside kira.txt.
![](Screenshots/31_kira_txt.png)
![](Screenshots/32_misa.png)

Switched to root using same password:

```bash
su root
```
![](Screenshots/33_root.png)

Read root.txt flag.
![](Screenshots/34_root_txt.png)

## 🧹 Post-Exploitation

Verified cleanup.

Captured all important flags: user.txt, kira.txt, root.txt.
![user.txt](Screenshots/23_user_txt.png)
![kira.txt](Screenshots/31_kira_txt.png)
![root.txt](Screenshots/34_root_txt.png)

## 🎉 Root Flag

Congratulations! You’ve successfully rooted the Death Note machine!

---

## 📋 Summary

We exploited a WordPress-based web application and used a brute-force attack with `Hydra` to get SSH credentials. Then, using lateral movement and privilege escalation tactics, we accessed and read protected files to ultimately gain root access.

---

## ✅ Recommendations

- Hide Sensitive Files: Remove public access to files like notes.txt, user.txt, and kira.txt.
- Use Strong Passwords: Avoid weak, guessable credentials; enforce strong password policies.
- Harden WordPress: Keep plugins updated, limit user enumeration, and add login protection.
- Set Proper File Permissions: Restrict file access using least privilege.
- Secure SSH: Prefer SSH keys over passwords and restrict login attempts.
- Limit Information Leakage: Avoid exposing internal hints or unnecessary details.

---

## 🔚 Conclusion
This lab emphasized:

- Manual enumeration techniques
- WordPress exploitation
- Credential brute-forcing with Hydra
- Steganography and CyberChef analysis
- Privilege escalation via weak passwords

---

