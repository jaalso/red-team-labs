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

**nmap version scan — identifying vsftpd 2.3.4**
<br><img width="611" height="167" alt="image" src="https://github.com/user-attachments/assets/d51c3811-2c13-4a6e-8a3d-8f0e622d4b43" />

**Metasploit — root shell obtained**
<br><img width="531" height="117" alt="image" src="https://github.com/user-attachments/assets/a8a17a0f-5d3e-42d8-8ba0-347b4e12013e" />
<br><img width="401" height="215" alt="image" src="https://github.com/user-attachments/assets/33cb446c-2d39-4a2c-9f3f-dcbf43f040d4" />


> 📄 **[Download Full Lab Report (PDF)](https://github.com/jaalso/cybersecurity-portfolio/raw/main/Pentest_Lab_Writeup_protected.pdf)**
<br>🔒 Password protected — contact me via [LinkedIn](https://linkedin.com/in/jaalso) to request access

---
### GoPhish: Phishing Simulation 
This lab provided hands-on experience with the complete phishing simulation lifecycle and
reinforced several key concepts from the email security module.
<br>**Tools:** GoPhish · Postfix · Gmail SMTP · Ngrok · CyberChef

Infrastructure Setup
- ✅ GoPhish v0.12.1 deployed on Kali Linux VM — admin panel at https://127.0.X.X:XXXX
- ✅ Gmail SMTP configured as authenticated relay with App Password
- ✅ Ngrok tunnel exposing local landing page publicly without a purchased domain
- ✅ DNS persistence fix — /etc/resolv.conf locked with chattr +i to survive sudo sessions
- ✅ Delivery verified with swaks before configuring GoPhish
  
Campaign Configuration
- ✅ Sending profile — Gmail SMTP with App Password authentication
- ✅ Email template v1 — custom Google security alert with {{.FirstName}} and {{.URL}} variables
- ✅ Email template v2 — real Google security alert cloned using CyberChef quoted-printable decode + regex URL replacement → indistinguishable from legitimate email
- ✅ Landing page — custom Google login form with credential capture enabled
- ✅ Users & Groups — targeted test account with CEO role for realistic pretext

**Campaign Results**
<br><img width="867" height="939" alt="image" src="https://github.com/user-attachments/assets/c6c98bc5-364c-4864-97fa-38bad07c9ddd" />
<br><img width="862" height="549" alt="image" src="https://github.com/user-attachments/assets/45dcf5f6-cdb7-4291-8dc1-beb906bac0ae" />

**Real Email Template Cloning**
<br><img width="768" height="586" alt="image" src="https://github.com/user-attachments/assets/fea054df-a78a-47bb-baf5-5f8cfa79292f" />

> 📄 **[Download Full Lab Report (PDF)](https://github.com/jaalso/cybersecurity-portfolio/raw/main/gophish_lab_report_protected.pdf)**
<br>🔒 Password protected — contact me via [LinkedIn](https://linkedin.com/in/jaalso) to request access

GoPhish lab successfully demonstrated a complete phishing simulation from infrastructure
setup through email delivery, link click tracking, and landing page credential capture. All five
components of a GoPhish campaign were configured and tested:
- Sending Profile — Gmail SMTP with App Password authentication
- Email Template — custom HTML and real Google email clone
- Landing Page — custom Google login form with credential capture
- Users & Groups — targeted test account with role information
- Campaign — launched, tracked, and results recorded




## Tools Used

| Category | Tools |
|---|---|
| Scanning & Recon | nmap · netdiscover · Wireshark |
| Exploitation | Metasploit · Hydra · CrackMapExec |
| Phishing | GoPhish · emlAnalyzer · CyberChef |
| Platform | Kali Linux · Metasploitable 2 · VirtualBox |

---

## ⚖️ Legal & Ethical Notice

All offensive security activities were conducted exclusively in:
- Isolated VirtualBox lab environments (no external connectivity)
- Authorised external targets (vuln.land)
- Training platforms (TryHackMe, HackTheBox)

No unauthorised systems were accessed. All work complies with Swiss law.
