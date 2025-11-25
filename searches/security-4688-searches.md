# Security Log Searches (Event ID 4688)

These are the Splunk searches used to analyze Windows Security process creation events (EventCode 4688).

---

## 🔍 Basic 4688 Search

```spl
index=* EventCode=4688
```

---

## 🔍 Process Creation + Command Line Parameters

```spl
index=* EventCode=4688 | table _time, New_Process_Name, Creator_Process_Name, Command_Line, Security_ID
```

---

## 🔍 Detect High-Risk Parent Processes

```spl
index=* EventCode=4688 
| search Creator_Process_Name IN ("powershell.exe", "cmd.exe", "mshta.exe", "wscript.exe", "cscript.exe")
| table _time, Creator_Process_Name, New_Process_Name, Command_Line
```

---

## 🔍 Look for Suspicious LOLBins

```spl
index=* EventCode=4688 
| search New_Process_Name IN ("certutil.exe", "regsvr32.exe", "rundll32.exe", "bitsadmin.exe")
| table _time, New_Process_Name, Command_Line
```

---

## 🔍 Look for Executables Spawned from Downloads Folder

```spl
index=* EventCode=4688 
| search Command_Line="*Downloads*"
| table _time, New_Process_Name, Command_Line
```

These serve as your first building blocks for basic detection work.

