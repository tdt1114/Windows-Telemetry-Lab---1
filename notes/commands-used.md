# Commands Used in Windows Telemetry Lab – Phase 1

This document contains all of the key commands used during the setup, validation, and troubleshooting of the Windows Telemetry Lab.

---

## 🔧 Sysmon Installation & Validation

```powershell
.\Sysmon64.exe -i -accepteula
```

Check Sysmon service status:

```powershell
Get-Service sysmon
```

List Sysmon log channel:

```powershell
Get-WinEvent -ListLog *sysmon*
```

View Sysmon events:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'} -MaxEvents 5
```

---

## 🔐 Enable Process Creation Logging (4688)

Enable auditing:

```powershell
auditpol /set /subcategory:"Process Creation" /success:enable
```

Enable command-line logging:

```powershell
reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\Audit /v ProcessCreationIncludeCmdLine_Enabled /t REG_DWORD /d 1 /f
```

Check Security event log for 4688:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4688} -MaxEvents 5
```

---

## 📊 Splunk Commands

Check Splunk status:

```powershell
cd "C:\Program Files\Splunk\bin"
.\splunk status
```

Restart Splunk:

```powershell
.\splunk restart
```

Stop Splunk:

```powershell
.\splunk stop
```

Start Splunk:

```powershell
.\splunk start
```

Clean event data (used during service corruption recovery):

```powershell
.\splunk clean eventdata
```

---

## 🛜 Generate Test Events

Open Notepad (process creation):

```powershell
notepad
```

Network connection:

```powershell
ping google.com
```

User identity (simple process):

```powershell
whoami
```

