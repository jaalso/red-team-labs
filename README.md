> 🧰Offensive security lab write-ups  
 
All labs conducted in isolated VirtualBox environments or on authorised external targets.
No unauthorised systems were accessed. All work complies with Swiss law and ethical hacking standards.

---

## 📁 Labs

### 01 · Network Penetration Testing Lab
**Tools:** nmap · Metasploit · Hydra · Wireshark  
**Target:** Metasploitable 2 (isolated VirtualBox lab)

Full kill-chain penetration test from network discovery to root shell.

- ✅ Network scanning with nmap (SYN, version, OS detection)
- ✅ Exploitation via Metasploit — CVE-2011-2523 (vsftpd backdoor), CVE-2007-2447 (Samba RCE)
- ✅ Root shell obtained — uid=0(root) confirmed
- ✅ Post-exploitation: /etc/shadow extraction, full filesystem enumeration
- ✅ Kill chain documented across all 7 MITRE ATT&CK phases
- ✅ Secondary backdoor on port 1524 (netcat bindshell) identified

📄 **[Download Full Lab Report (PDF)](https://github.com/jaalso/cybersecurity-portfolio/raw/main/Pentest_Lab_Writeup_protected.pdf)**  
> 🔒 Password protected — contact me via [LinkedIn](https://linkedin.com/in/jaalso)

---

### 02 · GoPhish Phishing Simulation
**Tools:** GoPhish · Kali Linux · SMTP  
**Status:** 🔜 Write-up coming soon

Controlled phishing simulation campaign testing user awareness and measuring click/credential rates.

- ✅ Email template creation and spoofing techniques
- ✅ SMTP configuration and DNS record analysis (SPF, DKIM, DMARC)
- ✅ Campaign tracking and results reporting

---

### 03 · Offensive Email Attack Chain
**Tools:** GoPhish · emlAnalyzer · CyberChef · MXToolbox  
**Status:** 🔜 Write-up coming soon

End-to-end offensive email attack chain documentation.

- ✅ Header analysis and spoofing detection
- ✅ MIME structure and attachment weaponisation
- ✅ IOC identification across the full delivery chain

---

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
