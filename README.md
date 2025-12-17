# Phishing Unfolding: Real-Time Incident Response Lab 🛡️

A comprehensive documentation of a high-pressure, live phishing attack simulation. This project demonstrates the ability to monitor SIEM alerts, analyze malicious process trees, and detect advanced exfiltration techniques like DNS tunneling.



## 📋 Scenario Overview
In this simulation, I acted as a **SOC Analyst** tasked with defending a corporate network. The project involved distinguishing between benign system noise and actual malicious indicators (IoCs) to reconstruct a full attack chain.

## ☣️ The Attack Chain (Kill Chain)

### 1. Delivery & Initial Access
* **Vector:** Phishing email from `eileen@trendymillineryco.me` (Suspicious TLD).
* **Payload:** A malicious ZIP file `ImportantInvoice-Febrary.zip` containing a script that initiated a reverse shell.
* **Victim:** `michael.ascot@tryhackme.com` on workstation `win-3450`.

### 2. Execution & Persistence
* **Malicious Process:** `powershell.exe` (PID 3728) was spawned immediately after the user opened the attachment.
* **Tooling:** The attacker downloaded and executed **PowerView.ps1**, a common tool for Active Directory enumeration and discovery.

### 3. Lateral Movement & Data Staging
* **Share Discovery:** Use of `net.exe` to map a sensitive financial directory: `\\FILESRV-01\SSF-FinancialRecords`.
* **Staging:** The attacker used `Robocopy.exe` to move targeted financial workbooks into a local hidden directory: `C:\Users\michael.ascot\downloads\exfiltration\`.

### 4. Data Exfiltration (DNS Tunneling)
* **Method:** The attacker utilized `nslookup.exe` to bypass traditional firewalls.
* **Detection:** Identified high-frequency DNS queries to `haz4rdw4re.io`. 
* **Base64 Analysis:** Decoded the subdomains in the DNS queries, which revealed the actual content of stolen files (e.g., `Summary.xlsx`).



## 🛠️ Skills & Tools Demonstrated
* **SIEM Analysis:** Triaging alerts and identifying **True Positives** vs. **False Positives** (e.g., dismissing `TrustedInstaller.exe` noise).
* **Process Tree Investigation:** Analyzing parent-child process relationships to find the root cause of an infection.
* **Threat Hunting:** Detecting unauthorized administrative tools used for malicious purposes.
* **Incident Reporting:** Creating actionable intelligence for management and technical teams.

## 🚀 Remediation Strategy
Based on the analysis, the following immediate actions were recommended:
1.  **Isolate:** Disconnect workstation `win-3450` from the network to stop the reverse shell.
2.  **Block:** Enterprise-wide sinkholing of the C2 domain `haz4rdw4re.io`.
3.  **Preserve:** Capture all DNS logs as they contain the forensic fragments of the exfiltrated data.
4.  **Reset:** Forced credential reset for the compromised user `michael.ascot`.

---
*Developed as part of my Cybersecurity specialization. For more information, contact me at mhumdanwerdw@gmail.com.*
