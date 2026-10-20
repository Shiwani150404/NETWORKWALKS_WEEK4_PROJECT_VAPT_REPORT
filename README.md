# NETWORKWALKS_WEEK4_PROJECT_VAPT_REPORT

# 🏥 Penetration Testing Report — Mediroza General Hospital

> **NetworkWalks Cybersecurity & Ethical Hacking Programme | Week 04 | Batch B083**

---

## 📌 Overview

This repository documents a full black-box penetration test conducted against a training target, `medirozahospital.com`, as part of Week 4 of my Cybersecurity & Ethical Hacking programme at NetworkWalks. Unlike earlier weeks (single-tool exercises), this project chains together reconnaissance, web application exploitation, password cracking, and metadata analysis into one complete attack path — mirroring how a real-world penetration test unfolds.

The full write-up is available in report
---

## 🚨 Scope & Liability Disclaimer

> This engagement was conducted under written authorisation granted by NetworkWalks for every milestone, against a designated training target built for this exercise. These techniques were applied only within that authorised scope and must never be used against any system without explicit written permission from its owner.

---

## 🛠️ Tools Used

| Category | Tool | Purpose |
|---|---|---|
| Recon | whois, nslookup, whatweb, curl | Domain/DNS/web fingerprinting |
| Enumeration | gobuster, theHarvester, wpscan | Directory brute-forcing and OSINT |
| Exploitation | Manual testing (browser) | Username enumeration & SQL injection |
| Password Cracking | NetworkWalks Hash Calculator & Password Cracker | Dictionary attacks on encrypted PDFs |
| Forensics | qpdf, exiftool | Decryption and metadata analysis |
| Data Retrieval | wget | Downloading exposed server files |

---

## 🔍 Attack Chain Summary

1. **Reconnaissance** — `robots.txt` disclosed three hidden folders: `/patient/`, `/staff/`, `/old/`.
2. **Authentication flaw** — The `/patient/login.php` form leaked whether a username existed via different error messages (username enumeration), and was vulnerable to SQL injection, allowing a full login bypass with the payload `admin' --`.
3. **Data exposure (Stage 1)** — The bypassed login exposed 3 confidential, password-protected patient report PDFs.
4. **Password cracking** — Two PDFs fell to a small common-password wordlist; the third required escalating to a larger wordlist, demonstrating how password strength directly affects crack time.
5. **Metadata leak** — Hidden metadata in one PDF (Author + an internal comment) pointed to a forgotten backup folder — the same `/old/` folder flagged all the way back in recon.
6. **Data exposure (Stage 2)** — `/old/` had directory listing enabled, exposing a full database backup containing confidential HR and shareholder records in plain text.

---

## ⚠️ Risk & Impact Summary

| # | Finding | Risk Level |
|---|---|---|
| 1 | Username enumeration on login page | Medium |
| 2 | SQL injection — authentication bypass | **Critical** |
| 3 | Encrypted PDFs accessible after bypass | High |
| 4 | Weak, dictionary-crackable PDF passwords | High |
| 5 | Sensitive metadata left in patient PDFs | Medium |
| 6 | Forgotten backup folder, directory listing enabled | **Critical** |
| 7 | Confidential HR/shareholder data exposed in plain text | **Critical** |

*Full evidence, methodology, and remediation recommendations are in the attached report.*

---

## 💡 Key Recommendations

- Return identical, generic error messages on login failures regardless of which field was wrong.
- Use parameterised queries / prepared statements — never concatenate raw input into SQL.
- Store sensitive files outside the public web root, behind proper access controls.
- Strip metadata from files before distribution (`exiftool -all= filename.pdf`).
- Disable directory listing server-wide; never store backups inside the web root.

---

## 📄 Project Metadata

- **Author:** Shiwani Dodke
- **Programme:** Cybersecurity & Ethical Hacking, NetworkWalks (Batch B083)
- **Week:** 04 | **Client:** Mediroza General Hospital
- **Engagement Type:** Black-box Penetration Test
