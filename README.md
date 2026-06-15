# TryHackMe — Boogeyman 3
**SOC Level 1 Pathway | Incident Response Investigation**

## Overview
Full kill chain analysis of a multi-stage attack against Quick Logistics LLC. 
The threat actor gained initial access via a phishing lure, established persistence, 
moved laterally across the environment, compromised the domain controller, and staged ransomware.

**Timeframe:** August 29–30, 2023  
**Tools:** Elastic/Kibana, Sysmon, KQL, Windows Event Logs  
**Victim Host:** WKSTN-0051 | **User:** evan.hutchinson

---

## Attack Chain Summary

| # | Stage | TTP |
|---|-------|-----|
| 1 | Initial Access | HTA lure disguised as PDF via mounted ISO — executed by mshta.exe |
| 2 | Payload Implant | xcopy dropped review.dat (DLL) to Temp folder |
| 3 | Execution | rundll32.exe executed review.dat via DllRegisterServer |
| 4 | Persistence | Scheduled task "Review" created — runs daily at 06:00 |
| 5 | C2 | rundll32 beaconing to 165.232.170.151:80 |
| 6 | Privilege Escalation | UAC bypass via fodhelper.exe |
| 7 | Credential Dumping | Mimikatz downloaded from GitHub — sekurlsa::logonpasswords |
| 8 | Credential Access | itadmin NTLM hash dumped from WKSTN-0051 |
| 9 | Discovery | IT_Automation.ps1 read from \\WKSTN-1327\ITFiles share |
| 10 | Credential Access | Plaintext credentials for allan.smith found in script |
| 11 | Lateral Movement | Invoke-Command to WKSTN-1327 via WinRM (wsmprovhost.exe) |
| 12 | Credential Dumping | administrator NTLM hash dumped from WKSTN-1327 |
| 13 | DC Compromise | DCSync attack — backupda account dumped |
| 14 | Ransomware Staging | ransomboogey.exe downloaded from ff.sillytechninja.io |

---

## Key IOCs

| Type | Value |
|------|-------|
| File | ProjectFinancialSummary_Q3.pdf.hta |
| File | review.dat (DLL) |
| IP:Port | 165.232.170.151:80 |
| URL | hxxp[://]ff[.]sillytechninja[.]io/ransomboogey[.]exe |
| Hash (NTLM) | F84769D250EB95EB2D7D8B4A1C5613F2 (itadmin) |
| Hash (NTLM) | 00f80f2538dcb54e7adc715c0e7091ec (administrator) |
| Scheduled Task | Review |

---

## Detailed Report
See [Boogeyman3_Attack_Chain.pdf](./Boogeyman3_Attack_Chain.pdf) for the full analysis.

---

*TryHackMe SOC Level 1 Pathway — Boogeyman 3 | Analyst: czabatta*
