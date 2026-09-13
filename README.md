# HackTheBox — Starting Point Writeups

A collection of detailed, professional walkthroughs and writeups for the **Starting Point** series on [HackTheBox](https://www.hackthebox.com/). This repository documents the methodology, tools, commands, and security takeaways for each machine solved.

---

## 📋 Completed Machines

| # | Machine | Tier | OS | Difficulty | Primary Vector / Service | Walkthrough |
|---|---|---|---|---|---|---|
| 1 | **Fawn** | Tier 0 | Linux | Very Easy | vsftpd 3.0.3 — Anonymous Authentication | [Read Writeup](./HackTheBox%20-%20Fawn%20%28Starting%20Point%29.md) |

---

## 🛠️ Tooling & Methodology

The walkthroughs in this repository emphasize standard penetration testing methodologies:

1. **Reconnaissance & Port Scanning:** `nmap` (TCP SYN stealth scans, service detection, OS detection, NSE scripts).
2. **Vulnerability Assessment:** Identifying misconfigurations, unauthenticated access points, and software CVEs.
3. **Exploitation & Initial Access:** Interacting with vulnerable network services using native clients and exploitation tools.
4. **Post-Exploitation & Flag Capture:** Locating and exfiltrating target flags (`flag.txt`).
5. **Remediation & Hardening:** Practical defensive guidance to mitigate identified vulnerabilities.

---

## 📁 Repository Structure

```text
.
├── HackTheBox - Fawn (Starting Point).md  # Detailed Fawn walkthrough
├── Image Asset/                          # Evidence screenshots and scan outputs
│   ├── Pasted image 20260913212841.png
│   ├── Pasted image 20260913212903.png
│   ├── Pasted image 20260913213345.png
│   ├── Pasted image 20260913213425.png
│   └── Pasted image 20260913213556.png
├── README.md                             # Repository index & summary
└── .gitignore                            # Git ignore rules
```

---

## ⚖️ Disclaimer

All walkthroughs, code, and documentation in this repository are produced for educational and cybersecurity training purposes only. Techniques described should only be executed in controlled lab environments or on systems where explicit authorization has been granted.
