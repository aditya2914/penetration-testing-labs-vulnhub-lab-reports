# Jangow: 1.0.1 – VulnHub Write-up

## ✅ Objective
Enumerate, exploit, and escalate privileges to gain full root access on the **Jangow: 1.0.1** VulnHub machine.

---

## 🧠 Skills Demonstrated
- Full-port and service enumeration
- Web directory discovery and analysis
- Sensitive file exposure exploitation
- Database credential extraction
- Linux privilege escalation
- Methodical penetration testing workflow

---

## 🏗 Environment
- Attacker: Kali Linux
- Target: Jangow: 1.0.1 (VirtualBox)
- Network: Host-only / 192.168.56.0/24

---

## 🔍 Enumeration

### ✅ Network Discovery
```bash
nmap -sS -p- <target-ip>
