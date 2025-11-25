# Sysmon Searches (Event ID 1 & 3)

These searches are used once Sysmon → Splunk ingestion is connected (Phase 2). Sysmon is already confirmed working at the OS level during Lab 1.

---

## 🔍 Sysmon Process Creation (Event ID 1)

```spl
index=* EventID=1
| table _time, Image, ParentImage, CommandLine, User
```

---

## 🔍 Sysmon Network Connections (Event ID 3)

```spl
index=* EventID=3
| table _time, Image, DestinationIp, DestinationPort, Protocol
```

---

## 🔍 Sysmon Process Ancestry (Useful for Detections)

```spl
index=* EventID=1
| stats min(_time) as first_seen by ParentImage, Image
| sort first_seen
```

---

## 🔍 Detect PowerShell Spawning Suspicious Binaries

```spl
index=* EventID=1
| search ParentImage="*powershell.exe"
| table _time, ParentImage, Image, CommandLine
```

---

## 🔍 Detect Abnormal LOLBins in Sysmon

```spl
index=* EventID=1
| search Image IN ("*certutil.exe", "*regsvr32.exe", "*mshta.exe", "*rundll32.exe")
| table _time, Image, CommandLine
```

These will be used in Lab 2 when you start building your first detection logic with Sysmon data.
