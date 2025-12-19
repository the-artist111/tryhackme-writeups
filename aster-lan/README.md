# 🛡️ TryHackMe — Aster LAN Write-up

**Room Name:** Aster LAN  
**Platform:** TryHackMe  
**Difficulty:** Intermediate  
**Focus:** Enumeration & Service Analysis  
**Author:** the-artist111  

> ⚠️ All activities were performed on a legal, intentionally vulnerable TryHackMe lab.

---

## 🎯 Objective

The goal of this lab was to enumerate a target machine inside a local network, identify exposed services, and analyze potential attack paths based on discovered services.

---

## 🧠 Methodology

1. Network Enumeration  
2. Service Identification  
3. Web Application Analysis  
4. Service-Specific Investigation  
5. Documentation of Findings

---

## 🔍 Initial Enumeration

The first step was to identify open ports and running services using **Nmap**.  
A full TCP scan was performed to avoid missing non-standard services.

### 🔧 Command Used

```bash
sudo nmap -sC -sV -p- -oN aster_full_scan.txt <TARGET-IP>
📄 Scan Results (Key Findings)
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu
80/tcp   open  http        Apache httpd 2.4.18
1720/tcp open  h323q931
2000/tcp open  cisco-sccp
5038/tcp open  asterisk    Asterisk Call Manager 5.0.2

🧠 Analysis

Port 22 (SSH): Available but requires credentials — no immediate access.

Port 80 (HTTP): Primary attack surface for enumeration.

Port 5038 (Asterisk): Interesting service often misconfigured.

Other telephony-related ports indicate a VoIP environment.

🌐 Web Application Enumeration (Port 80)

The web service was accessed via browser to inspect available content.

http://<TARGET-IP>


The page appeared minimal, suggesting possible hidden directories or functionality.

📁 Directory Enumeration

To discover hidden endpoints, Gobuster was used.

🔧 Command Used
gobuster dir -u http://<TARGET-IP> -w /usr/share/wordlists/dirb/common.txt -o gobuster_results.txt

📄 Result Summary

No high-value directories discovered using the common wordlist

Indicates either:

Custom paths

Service-based exploitation instead of web-based

🔐 SSH Service Review (Port 22)

SSH access was confirmed but required valid credentials.🔐 SSH Service Review (Port 22)

SSH access was confirmed but required valid credentials.

ssh user@<TARGET-IP>


At this stage, brute-forcing was avoided to stay within ethical testing practices.
SSH was noted as a potential later entry point if credentials were discovered.

📞 Asterisk Service Analysis (Port 5038)

The Asterisk Call Manager service was identified as an uncommon but potentially vulnerable service.

🔧 Service Detection Command
nmap -p 5038 --script=asterisk-scan <TARGET-IP>

🧠 Why This Matters

Asterisk services sometimes expose:

Default credentials

Misconfigured management interfaces

Information disclosure

This service would require protocol-specific investigation beyond standard web testing.

🧪 Key Observations
Finding	Security Impact
Multiple open services	Increased attack surface
Minimal web content	Possible hidden logic or non-web exploitation
Asterisk running	Specialized service often misconfigured
SSH available	Potential post-credential access
🧠 Lessons Learned

Always perform a full port scan

Do not rely only on web exploitation

Specialized services (VoIP, Asterisk) deserve manual investigation

Enumeration quality directly impacts exploitation success

🛠 Tools Used

Nmap — Port scanning & service detection

Gobuster — Directory brute forcing

Browser / Curl — Manual inspection

SSH — Remote access testing

📌 Conclusion

This lab emphasized the importance of thorough enumeration and recognizing non-standard services. While no immediate exploitation path was available early, identifying and understanding exposed services is critical for advanced penetration testing scenarios.

Future steps would involve deeper service interaction and credential discovery.




