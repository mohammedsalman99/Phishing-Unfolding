Phishing Unfolding: Real-Time Incident Response Lab

This repository documents a high-pressure, live phishing attack scenario where I acted as a SOC Analyst to identify, analyze, and document each phase of a corporate breach as it unfolded.


🎯 Scenario Overview
The project involves monitoring real-time SIEM alerts within a corporate network to piece together an attack chain. The goal was to differentiate between benign system noise and malicious activities like PowerShell executions, reverse shells, and data exfiltration.




🛡️ Incident Timeline & Analysis
Phase 1: Initial Access (Phishing)

Alert ID 1000: A suspicious email was received from an external domain (eileen@trendymillineryco.me) using an unusual TLD (.me).




Analysis: Classified as a True Positive due to known phishing patterns (advance-fee scam) and requests for banking details.




Alert ID 1005: A more dangerous email was received containing a malicious attachment: ImportantInvoice-Febrary.zip.


Phase 2: Post-Exploitation & Reconnaissance

PowerShell Execution: 22 minutes after the malicious ZIP arrived, the user (michael.ascot) executed the payload, which dropped PowerView.ps1 into the Downloads folder.


Reconnaissance: The attacker used a persistent PowerShell process (PID 3728) to perform Active Directory reconnaissance and domain enumeration.




Phase 3: Lateral Movement & Data Staging

Drive Mapping: The attacker used net.exe to silently map a sensitive financial share (\FILESRV-01\SSF-FinancialRecords) to the Z: drive.


Data Staging: Robocopy.exe was used to recursively copy all financial records into a local exfiltration folder: C:\Users\michael.ascot\downloads\exfiltration.

Phase 4: Exfiltration via DNS Tunneling

Mechanism: The attacker utilized nslookup.exe to leak data chunk-by-chunk.


Indicators: Base64-encoded strings representing actual file headers (e.g., Summary.xlsx) were observed in DNS queries to the malicious domain haz4rdw4re.io.


🛠️ Skills & Tools Demonstrated

Log Analysis: Investigating Sysmon and SIEM alerts to trace parent-child process relationships.


Threat Hunting: Identifying offensive tools like PowerView and unauthorized use of administrative tools like Robocopy.



Traffic Analysis: Recognizing DNS tunneling and C2 beaconing patterns.



Incident Reporting: Drafting comprehensive remediation actions, including network isolation and enterprise-wide domain blocking.



📋 Key Lessons: Filtering "Normal Noise"
A critical part of this lab was identifying False Positives to avoid "alert fatigue," such as:


Alert ID 1001: TrustedInstaller.exe launched by services.exe (Normal Windows Update behavior).



Alert ID 1006: rdpclip.exe launched by svchost.exe (Normal Remote Desktop session behavior).
