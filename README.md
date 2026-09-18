# HackTheBox â€” Starting Point Writeups

A collection of detailed, professional walkthroughs and writeups for the **Starting Point** series on [HackTheBox](https://www.hackthebox.com/). This repository documents the methodology, tools, commands, and security takeaways for each machine solved.

---

## ðŸ“‹ Completed Machines

### Tier 0

| # | Machine | Tier | OS | Difficulty | Primary Vector / Service | Walkthrough |
|---|---|---|---|---|---|---|
| 1 | **Meow** | Tier 0 | Linux | Very Easy | Telnet (`Linux telnetd`) â€” Blank Root Password | [Read Writeup](./HackTheBox%20-%20Meow%20%28Starting%20Point%29.md) |
| 2 | **Fawn** | Tier 0 | Linux | Very Easy | vsftpd 3.0.3 â€” Anonymous Authentication | [Read Writeup](./HackTheBox%20-%20Fawn%20%28Starting%20Point%29.md) |

### Tier 2

| # | Machine | Tier | OS | Difficulty | Primary Vector / Service | Walkthrough |
|---|---|---|---|---|---|---|
| 1 | **Archetype** | Tier 2 | Windows | Very Easy | SMB Anonymous Share -> MSSQL (`xp_cmdshell`) -> PSReadLine Credential Leak -> Impacket `psexec` | [Read Writeup](./HackTheBox%20-%20Archetype%20%28Starting-Point%20Tier%202%29.md) |

---

## ðŸ› ï¸ Tooling & Methodology

The walkthroughs in this repository emphasize standard penetration testing methodologies:

1. **Reconnaissance & Port Scanning:** `nmap` (TCP SYN stealth scans, all-ports sweeps, service detection, OS detection, NSE scripts).
2. **Vulnerability Assessment:** Identifying misconfigurations, unauthenticated access points, and software CVEs.
3. **Exploitation & Initial Access:** Interacting with vulnerable network services using native clients (`telnet`, `ftp`, `smbclient`), database tools (`impacket-mssqlclient`), and administrative execution utilities (`impacket-psexec`).
4. **Post-Exploitation & Privilege Escalation:** Harvesting credentials from configuration files and command history (`PSReadLine`), escalating privileges to `SYSTEM`, and exfiltrating target flags (`user.txt`, `root.txt`).
5. **Remediation & Hardening:** Practical defensive guidance to mitigate identified vulnerabilities in production environments.

---

## ðŸ“ Repository Structure

```text
.
â”œâ”€â”€ HackTheBox - Archetype (Starting-Point Tier 2).md # Detailed Archetype walkthrough (SMB & MSSQL)
â”œâ”€â”€ HackTheBox - Fawn (Starting Point).md             # Detailed Fawn walkthrough (FTP)
â”œâ”€â”€ HackTheBox - Meow (Starting Point).md             # Detailed Meow walkthrough (Telnet)
â”œâ”€â”€ Image Asset/                                     # Evidence screenshots and scan outputs
â”‚   â”œâ”€â”€ Pasted image 20260913212841.png
â”‚   â”œâ”€â”€ Pasted image 20260913212903.png
â”‚   â”œâ”€â”€ Pasted image 20260913213345.png
â”‚   â”œâ”€â”€ Pasted image 20260913213425.png
â”‚   â”œâ”€â”€ Pasted image 20260913213556.png
â”‚   â”œâ”€â”€ Pasted image 20260916204338.png
â”‚   â”œâ”€â”€ Pasted image 20260916204412.png
â”‚   â”œâ”€â”€ Pasted image 20260916204443.png
â”‚   â”œâ”€â”€ Screenshot 2026-09-18 193729.png
â”‚   â”œâ”€â”€ Screenshot 2026-09-18 193853.png
â”‚   â”œâ”€â”€ Screenshot 2026-09-18 193934.png
â”‚   â”œâ”€â”€ Screenshot 2026-09-18 194021.png
â”‚   â”œâ”€â”€ Screenshot 2026-09-18 194232.png
â”‚   â”œâ”€â”€ Screenshot 2026-09-18 194320.png
â”‚   â”œâ”€â”€ Screenshot 2026-09-18 194430.png
â”‚   â””â”€â”€ Screenshot 2026-09-18 194558.png
â”œâ”€â”€ README.md                                        # Repository index & summary
â””â”€â”€ .gitignore                                       # Git ignore rules
```

---

## âš–ï¸ Disclaimer

All walkthroughs, code, and documentation in this repository are produced for educational and cybersecurity training purposes only. Techniques described should only be executed in controlled lab environments or on systems where explicit authorization has been granted.