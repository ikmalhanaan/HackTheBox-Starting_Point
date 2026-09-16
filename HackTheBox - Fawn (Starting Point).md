# HackTheBox — Fawn (Starting Point)

**Target IP:** `10.129.117.112`  
**Difficulty:** Very Easy  
**Tier:** Tier 0 (Starting Point)  
**Operating System:** Linux  
**Vulnerability / Service:** Insecure / Misconfigured FTP Service (vsftpd 3.0.3) — Anonymous Authentication  

---

## 1. Executive Summary & Objective

**Fawn** is an entry-level machine from HackTheBox Starting Point designed to introduce the fundamentals of network reconnaissance, port scanning with Nmap, and exploiting misconfigured network services. 

The objective of this challenge is to identify open ports, enumerate the underlying service versions, leverage misconfigured anonymous access on an FTP (File Transfer Protocol) daemon, and exfiltrate the `flag.txt` file stored on the remote target.

---

## 2. Methodology Overview

The penetration testing methodology for this target follows standard phases:
1. **Network Reconnaissance:** Fast SYN port scan across the standard top 1,000 TCP ports.
2. **Service & Version Enumeration:** Targeted inspection of discovered ports using Nmap Scripting Engine (NSE) and service version detection.
3. **Exploitation (Initial Access):** Authenticating to the vsftpd server using the default `anonymous` credentials.
4. **Data Exfiltration & Flag Capture:** Navigating the FTP directory hierarchy and downloading `flag.txt`.
5. **Mitigation & Security Recommendations:** Hardening FTP configurations against unauthorized access.

---

## 3. Phase 1: Reconnaissance (Port Scanning)

We initiate the reconnaissance phase by executing an aggressive, fast SYN stealth scan to identify active TCP ports on the target host.

```bash
sudo nmap -sS -T4 -Pn -n -v $TARGET -oA fast.nmap
```

### Command Flags Breakdown:
- `sudo`: Runs Nmap with root privileges, necessary for raw socket operations like SYN stealth scans.
- `-sS`: Performs a TCP SYN (Stealth) scan. It sends SYN packets and does not complete the 3-way handshake, reducing connection overhead and logging footprint.
- `-T4`: Sets the timing template to aggressive for faster scan execution without sacrificing reliability.
- `-Pn`: Skips ICMP host discovery, treating the target host as alive.
- `-n`: Disables reverse DNS resolution to reduce latency.
- `-v`: Enables verbose output to report discovered open ports in real-time.
- `-oA fast.nmap`: Saves the scan results in three major formats (`.nmap`, `.gnmap`, and `.xml`) with the basename `fast.nmap`.

### Scan Result:
![Initial Nmap Recon Scan](Image%20Asset/Pasted%20image%2020260913212841.png)

From the scan output, we identify an open port:
- **Port 21/TCP:** `open` — `ftp` (File Transfer Protocol)

All other 999 scanned ports returned closed (TCP RST).

---

## 4. Phase 2: Service & Vulnerability Enumeration

With port 21 identified as an active FTP service, we perform targeted service version detection and run default Nmap NSE scripts to detect misconfigurations or known vulnerabilities.

```bash
sudo nmap -sC -sV -O -p 21 -Pn -n -v $TARGET -oA service.nmap
```

### Command Flags Breakdown:
- `-sC`: Executes default Nmap scripts (equivalent to `--script=default`), checking for standard vulnerabilities, authentication behaviors, and banners.
- `-sV`: Probes open ports to determine service and version details.
- `-O`: Enables OS detection.
- `-p 21`: Scopes the scan exclusively to port 21.
- `-oA service.nmap`: Exports the findings to `service.nmap.*` output files.

### Scan Result:
![Service Version and NSE Scan](Image%20Asset/Pasted%20image%2020260913212903.png)

### Key Findings from Output:
```text
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
Service Info: OS: Unix
```

- **FTP Daemon:** `vsftpd 3.0.3` running on a Unix/Linux environment.
- **Critical Misconfiguration:** `ftp-anon: Anonymous FTP login allowed (FTP code 230)`.
- **Exposed Files:** Nmap's automated script detected a world-readable file `flag.txt` (32 bytes) located directly in the FTP root directory.

---

## 5. Phase 3: Exploitation (Anonymous FTP Access)

FTP servers configured with anonymous access allow users to authenticate without requiring dedicated user accounts or valid passwords. Commonly, providing the username `anonymous` or `ftp` with an empty or arbitrary password grants read access to designated public directories.

We connect to the remote FTP service using the standard Linux `ftp` client:

```bash
ftp 10.129.117.112
```

### Authentication Walkthrough:
1. When prompted for `Name`, provide `anonymous`.
2. When prompted for `Password`, leave it blank or enter any string (e.g., press `Enter`).
3. The server responds with `230 Login successful`, granting us an interactive FTP session.

![FTP Anonymous Login](Image%20Asset/Pasted%20image%2020260913213425.png)

---

## 6. Phase 4: Flag Retrieval & Exfiltration

Once authenticated inside the FTP shell:

1. **List Directory Contents:**
   ```text
   ftp> ls
   ```
   Output confirms the presence of `flag.txt`:
   ```text
   -rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
   ```

2. **Download the Flag:**
   Use the `get` command to transfer the file to the local attacking machine:
   ```text
   ftp> get flag.txt
   ```

3. **Terminate the Session:**
   Close the connection cleanly:
   ```text
   ftp> exit
   ```

4. **Read the Flag:**
   View the contents of the retrieved file on the local terminal:
   ```bash
   cat flag.txt
   ```

![FTP Exfiltration and Flag Capture](Image%20Asset/Pasted%20image%2020260913213345.png)

---

## 7. Phase 5: Machine Solved

The retrieved flag was submitted to the HackTheBox platform, successfully completing the **Fawn** machine.

![Fawn Solved Confirmation](Image%20Asset/Pasted%20image%2020260913213556.png)

---

## 8. Remediation & Hardening Guidelines

Allowing anonymous FTP access in a production environment introduces serious confidentiality risks, especially if sensitive configuration backups, credentials, or proprietary files reside in accessible directories.

To secure a `vsftpd` deployment:

1. **Disable Anonymous Login:**
   Open `/etc/vsftpd.conf` and ensure the anonymous access directive is disabled:
   ```ini
   anonymous_enable=NO
   ```

2. **Enforce Local User Authentication & Chroot:**
   Restrict FTP access to authenticated local users only, and lock users to their respective home directories:
   ```ini
   local_enable=YES
   chroot_local_user=YES
   ```

3. **Migrate to Secure Transfer Protocols:**
   FTP transmits credentials and data in plain text. It is strongly recommended to deprecate plain FTP in favor of **SFTP (SSH File Transfer Protocol)** or **FTPS (FTP over TLS/SSL)** to ensure all traffic is encrypted in transit.

4. **Firewall & Network Segmentation:**
   Restrict port 21 access using host-based firewalls (`iptables`, `ufw`) to authorized internal IP addresses only.

