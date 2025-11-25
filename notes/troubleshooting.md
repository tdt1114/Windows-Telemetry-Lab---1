# Troubleshooting Log – Windows Telemetry Lab (Phase 1)

This document captures major issues encountered during the lab and how each was resolved. This demonstrates real-world troubleshooting and endpoint/SIEM problem-solving skills.

---

## 🟥 1. VirtualBox ISO Boot Loop  
**Issue:** VM kept rebooting to Windows setup.  
**Cause:** ISO still attached in virtual optical drive.  
**Fix:**  
- Devices → Optical Drives → Remove disk from virtual drive  
- Or VM Settings → Storage → Remove ISO  
   
---

## 🟥 2. Event Viewer “snap-in failed”  
**Issue:** Event Viewer froze or refused to load.  
**Fixes:**  
- End `mmc.exe` from Task Manager  
- Delete corrupted file at `%appdata%\Microsoft\MMC\eventvwr`  
- Run:
    ```powershell
    sfc /scannow
    ```

---

## 🟥 3. Splunk Service Stuck on “Starting”  
**Issue:** Splunkd was stuck in the `Starting` state and UI wouldn't load.  
**Cause:** PID lock or corrupted startup.  
**Fix:**  
1. End splunk processes in Task Manager  
2. Delete PID files from:
    ```
    C:\Program Files\Splunk\var\run\splunk\
    ```
3. Run:
    ```powershell
    .\splunk stop
    .\splunk clean eventdata
    .\splunk start
    ```

---

## 🟥 4. Sysmon Not Showing in Splunk Local Event Log List  
**Issue:** Sysmon log channel didn’t appear in Splunk UI.  
**Findings:**  
- Sysmon *was* logging correctly (PowerShell confirmed).  
- Splunk’s Event Log channel discovery is limited without TA.  

**Resolution Plan:**  
- Sysmon ingestion will be implemented in Phase 2  
- Security 4688 logs confirmed ingested successfully  
- No block to completing Lab 1  

---

## 🟥 5. VirtualBox Clipboard Not Working  
**Issue:** Could not paste commands into VM  
**Fix:**  
- VM → Settings → General → Advanced → Shared Clipboard: **Bidirectional**  
- Install Guest Additions:
  ```
  Devices → Insert Guest Additions CD Image
  ```

---

## 🟥 6. Splunk Not Accessible (Web UI Down)  
**Issue:** `http://localhost:8000` not loading  
**Fixes:**  
- Verified `splunkd` is running  
- Restarted Splunk cleanly  
- Confirmed port 8000 not occupied using:
    ```powershell
    netstat -ano | findstr 8000
    ```

---

## Summary

Nearly every issue reflected a real-world scenario a SOC analyst or detection engineer faces:
- corrupted MMC tools  
- stuck SIEM services  
- log ingestion troubleshooting  
- VM tool limitations  
- telemetry validation steps  

Documenting these problems demonstrates strong problem-solving and operational realism.

