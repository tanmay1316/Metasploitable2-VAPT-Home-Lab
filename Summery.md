# Metasploitable2 VAPT Home Lab

## Project Overview
This project is a self-driven network penetration testing lab built on **Metasploitable2**, an intentionally vulnerable Linux virtual machine, to practice and document real-world vulnerability assessment and exploitation techniques. The goal was to identify, exploit, and formally report on multiple classes of network-service vulnerabilities — moving beyond web application testing (see my [OWASP Juice Shop project](#)) into infrastructure-level VAPT.

## Lab Setup
- **Attacker machine:** Kali Linux — `192.168.56.101`
- **Target machine:** Metasploitable2 — `192.168.56.102`
- **Network:** VirtualBox Host-Only Adapter (isolated lab network)
- **Tools used:** Nmap, Metasploit Framework, searchsploit, manual service clients (mysql, psql, smbclient)

## Methodology
1. **Reconnaissance** — Full port and service/version scan using `nmap -sV -sC -p-`
2. **Vulnerability Identification** — Cross-referenced service versions against searchsploit, Exploit-DB, and CVE databases; also manually tested for default/blank credentials and misconfigurations
3. **Exploitation** — Used Metasploit modules (or manual exploitation for misconfig-based issues) to gain shell/database access
4. **Verification** — Confirmed access level via standard proof commands (`id`, `whoami`, `SELECT current_user`, etc.)
5. **Documentation** — Recorded methodology, evidence, impact, and remediation for each finding in a formal VAPT report

## Vulnerabilities Identified & Exploited

| # | Service | Version | Vulnerability | CVE / CWE | Impact |
|---|---------|---------|----------------|-----------|--------|
| 1 | vsftpd | 2.3.4 | Malicious backdoor in source distribution | CVE-2011-2523 / CWE-506 | Remote root shell |
| 2 | Samba | 3.0.20-Debian | `usermap_script` command injection | CVE-2007-2447 / CWE-78 | Remote root shell |
| 3 | UnrealIRCd | 3.2.8.1 | Backdoored binary (trojaned download) | CVE-2010-2075 / CWE-506 | Remote root shell |
| 4 | MySQL | 5.0.51a | Blank root password | CWE-521 | Full database access (root) |
| 5 | PostgreSQL | 8.3 | Default credentials (`postgres:postgres`) | CWE-521 | Full database access |
| 6 | Apache Tomcat | 5.5 (Manager) | Default credentials → WAR file upload RCE | CWE-521 / CWE-434 | Remote code execution, root-equivalent shell |

## Key Findings
- **4 of 6** vulnerabilities resulted in direct **remote root access**, demonstrating critical real-world impact.
- Vulnerabilities spanned multiple weakness categories: **embedded backdoors, command injection, weak/default credentials, and unrestricted file upload** — showing range across vulnerability classes rather than repeating one technique.
- Recurring technical challenges included legacy SSL/TLS incompatibility with older database services (resolved using `--skip-ssl` for MySQL and `sslmode=disable` for PostgreSQL) and correct RHOSTS/LHOST configuration in Metasploit for reliable payload delivery.

## Deliverables in This Repository
- `VAPT_Report.pdf` — Full formal report (methodology, evidence, impact, remediation per finding)
- `/screenshots` — Proof-of-access evidence for each exploit
- `home_lab_progress_notes.md` — Raw working notes from the lab build

## Skills Demonstrated
Network reconnaissance and enumeration • Exploit research (searchsploit, Exploit-DB, CVE lookups) • Metasploit Framework (exploit/payload selection, RHOSTS/LHOST configuration) • Manual service exploitation (MySQL, PostgreSQL clients) • Vulnerability impact analysis • Formal VAPT reporting

## Author
**Tanmay Narayankar**
[LinkedIn](https://linkedin.com/in/tanmay-narayankar) • [GitHub](https://github.com/tanmay1316) • [TryHackMe](https://tryhackme.com/p/tanmay9407)
