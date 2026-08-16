# Metasploitable2 VAPT Home Lab

## Project Overview
A self-driven network penetration testing lab built on **Metasploitable2**, an intentionally vulnerable Linux virtual machine, to practice and formally document real-world vulnerability assessment and exploitation techniques. This project moves beyond web application testing (see my [OWASP Juice Shop VAPT project](https://github.com/tanmay1316/Web-Application-Penetration-Testing-Lab-OWASP-Juice-Shop)) into infrastructure-level penetration testing — covering six distinct services and four different vulnerability classes.

## Lab Setup
- **Attacker machine:** Kali Linux — `192.168.56.101`
- **Target machine:** Metasploitable2 — `192.168.56.102`
- **Network:** VirtualBox Host-Only Adapter (isolated lab network)
- **Tools used:** Nmap, Metasploit Framework, searchsploit, manual service clients (`mysql`, `psql`, browser-based Tomcat Manager)

## Methodology
1. **Reconnaissance** — Full port and service/version scan using `nmap -sV -sC -p-`
2. **Vulnerability Identification** — Cross-referenced service versions against searchsploit, Exploit-DB, and Metasploit's internal module search; also manually tested for default/blank credentials and misconfigurations
3. **Exploitation** — Used Metasploit modules (or manual exploitation for misconfiguration-based issues) to gain shell, database, or application-level access
4. **Verification** — Confirmed access level via standard proof commands (`id`, `whoami`, `getuid`, `SELECT current_user`, etc.)
5. **Documentation** — Recorded methodology, evidence, impact, and remediation for each finding in an individual, screenshot-backed report

## Vulnerabilities Identified & Exploited

| # | Service | Version | Vulnerability | CVE / CWE | Access Achieved | Report |
|---|---------|---------|----------------|-----------|------------------|--------|
| 1 | vsftpd | 2.3.4 | Malicious backdoor in source distribution | CVE-2011-2523 / CWE-506 | Remote root shell | [01_vsftpd_backdoor.md](./01_vsftpd_backdoor.md) |
| 2 | Samba | 3.0.20-Debian | `usermap_script` command injection | CVE-2007-2447 / CWE-78 | Remote root shell | [02_samba_usermap_script.md](./02_samba_usermap_script.md) |
| 3 | UnrealIRCd | 3.2.8.1 | Backdoored binary (trojaned download) | CVE-2010-2075 / CWE-506 | Remote root shell | [03_unrealircd_backdoor.md](./03_unrealircd_backdoor.md) |
| 4 | MySQL | 5.0.51a | Blank root password | CWE-521 | Full database access (root) | [04_mysql_blank_password.md](./04_mysql_blank_password.md) |
| 5 | PostgreSQL | 8.3.1 | Default credentials (`postgres:postgres`) | CWE-521 / CWE-798 | Full database access (superuser) | [05_postgresql_default_credentials.md](./05_postgresql_default_credentials.md) |
| 6 | Apache Tomcat | Manager (port 8180) | Default credentials → WAR file upload RCE | CWE-521 / CWE-434 | Remote code execution (tomcat55) | [06_tomcat_manager_rce.md](./06_tomcat_manager_rce.md) |

## Key Findings
- **4 of 6** vulnerabilities resulted in direct **remote root access**; the remaining two yielded full database compromise and service-account-level RCE respectively — a realistic spread of outcomes.
- Findings spanned **four distinct vulnerability classes**: embedded backdoors (vsftpd, UnrealIRCd), command injection (Samba), weak/default credentials (MySQL, PostgreSQL, Tomcat), and unrestricted file upload (Tomcat) — deliberately selected for range rather than exhaustively exploiting every open port.
- **Weak or default credentials were the single most common root cause**, responsible for 3 of the 6 findings — a pattern that mirrors real-world breach data more closely than memory-corruption exploits do.
- Recurring technical challenges included legacy SSL/TLS incompatibility with older database services (resolved using `--skip-ssl` for MySQL, direct `psql` connection for PostgreSQL) and correctly configuring `RHOSTS`/`LHOST` in Metasploit for reliable payload delivery.
- One finding (Tomcat) is documented precisely as **service-account access (`tomcat55`), not root** — reported accurately rather than overstated, consistent with real-world VAPT reporting standards.

## Repository Structure
- `01` – `06` — Individual VAPT finding reports (methodology, evidence, impact, remediation) for each exploited service
- `/screenshots` — Proof-of-access evidence supporting every report, referenced inline in each finding

## Skills Demonstrated
Network reconnaissance and enumeration • Exploit research (searchsploit, Exploit-DB, Metasploit module search) • Metasploit Framework (module configuration, payload delivery, session management) • Manual service exploitation (MySQL, PostgreSQL, Tomcat Manager) • Vulnerability impact analysis • Accurate, evidence-based VAPT reporting

## Author
**Tanmay Narayankar**
[LinkedIn](https://linkedin.com/in/tanmay-narayankar) • [GitHub](https://github.com/tanmay1316) • [TryHackMe](https://tryhackme.com/p/tanmay9407)
