# Windows Telemetry Lab – Phase 1  
## Process Creation Logging (4688) + Sysmon Verification + Splunk Ingestion

### 🎯 Objective  
Build a foundational Windows telemetry lab to understand how process creation data flows from an endpoint into a SIEM. This includes installing Sysmon, enabling Windows Security logging (Event ID 4688), and ingesting logs into Splunk Enterprise for analysis.

---

## 🧱 Environment Setup

**Platform:** VirtualBox  
**OS:** Windows 10 Home (unactivated, legal for lab use)  
**SIEM:** Splunk Enterprise (local install)  
**Telemetry Sources:**  
- Windows Security Logs (4688)  
- Sysmon (Event ID 1, 3) – verified in PowerShell  

---

## ⚙️ Sysmon Installation & Verification

### ✔ Installed Sysmon using:
```powershell
.\Sysmon64.exe -i -accepteula
```

### ✔ Verified service status:
```powershell
Get-Service sysmon
```

### ✔ Verified Sysmon log channel exists:
```powershell
Get-WinEvent -ListLog *sysmon*
```

### ✔ Verified Sysmon is generating events:
```powershell
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'} -MaxEvents 5
```

---

## 🔐 Windows Security Logging (4688) Enabled

Since Windows 10 Home does not include `secpol.msc`, auditing was enabled manually:

### ✔ Enable process creation auditing:
```powershell
auditpol /set /subcategory:"Process Creation" /success:enable
```

### ✔ Enable command-line logging:
```powershell
reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\Audit /v ProcessCreationIncludeCmdLine_Enabled /t REG_DWORD /d 1 /f
```

### ✔ Verified via PowerShell:
```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4688} -MaxEvents 5
```

---

## 📊 Splunk Setup & Ingestion

### ✔ Installed Splunk Enterprise locally  
### ✔ Resolved service hang & PID lock issues  
### ✔ Confirmed Splunkd running:
```powershell
cd "C:\Program Files\Splunk\bin"
.\splunk status
```

### ✔ Successfully ingested Windows Security logs:
- **EventCode=4688** visible in Splunk
- Confirmed with:
```
index=* EventCode=4688
```

### ⚠️ *Sysmon → Splunk ingestion TBD in Lab 2*  
Sysmon is logging correctly on Windows and visible in PowerShell, but ingestion into Splunk will be completed in the next lab to avoid blocking initial project completion.

---

## 📸 Screenshots

Screenshots included in `/images` folder for:

- Sysmon service running  
- PowerShell Sysmon log verification  
- 4688 event in Splunk  
- Splunk Web UI running  
- Security auditing commands  
- Troubleshooting steps (optional)

---

## 🧠 What I Learned

- How to install and validate Sysmon logging  
- How Windows generates and stores process creation telemetry  
- How to enable command-line auditing even on Windows Home  
- How to install, repair, and restart Splunk safely  
- Real-world troubleshooting:
  - Event Viewer snap-in failures  
  - Splunkd service stuck in "Starting"  
  - PID lock issues  
  - VirtualBox ISO/boot errors  
  - Clipboard integration not enabled by default  
- How data flows from endpoint → event logs → SIEM  
- Foundation for building detections based on process telemetry

---

## 🚀 Next Steps (Lab 2)

- Add Sysmon → Splunk ingestion  
- Build custom SPL searches  
- Compare Sysmon Event ID 1 vs. Windows 4688  
- Start identifying suspicious process behaviors  
- Begin first detection rule write-up  
