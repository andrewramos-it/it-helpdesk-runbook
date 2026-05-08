# IT Help Desk Runbook
**Author:** Andrew Ramos  
**Role Target:** IT Help Desk / Technical Support Tier 1–2  
**Last Updated:** May 2026

A structured reference guide for resolving the most common Tier 1 end-user support issues. Each entry follows a consistent format: symptoms, likely causes, step-by-step resolution, and escalation criteria. Built from real-world support experience across remote and in-person environments.

---

## Table of Contents
1. [User Cannot Connect to Wi-Fi](#1-user-cannot-connect-to-wi-fi)
2. [Password Reset / Account Lockout](#2-password-reset--account-lockout)
3. [Printer Not Found or Won't Print](#3-printer-not-found-or-wont-print)
4. [Computer Running Slowly](#4-computer-running-slowly)
5. [Microsoft 365 / Outlook Not Working](#5-microsoft-365--outlook-not-working)
6. [Blue Screen of Death (BSOD)](#6-blue-screen-of-death-bsod)
7. [User Cannot Access a Shared Drive or Network Folder](#7-user-cannot-access-a-shared-drive-or-network-folder)

---

## 1. User Cannot Connect to Wi-Fi

**Symptoms:**
- Device shows "No Internet" or "Connected, no internet"
- Wi-Fi network not visible in available networks
- Connection drops repeatedly

**Likely Causes:**
- Incorrect credentials
- IP conflict or DHCP failure
- Adapter driver issue
- Router/access point issue

**Resolution Steps:**
1. Confirm the correct SSID is selected and credentials are correct
2. Run `ipconfig /release` then `ipconfig /renew` in Command Prompt (Windows)
3. Run `netsh winsock reset` and restart the device
4. Forget the network and reconnect from scratch
5. Check if other devices on the same network are affected — if yes, issue is on network side, not device
6. Update or roll back the network adapter driver via Device Manager
7. Test with a wired ethernet connection to isolate wireless vs. connectivity issue

**Escalate If:**
- Multiple users affected on the same network
- Router/switch access is needed
- Issue persists after all steps above

---

## 2. Password Reset / Account Lockout

**Symptoms:**
- User receives "incorrect password" or "account locked" error
- User cannot log into Windows, Microsoft 365, or company portal

**Likely Causes:**
- Forgotten password
- Caps Lock / keyboard layout issue
- Multiple failed login attempts triggering lockout policy
- Cached credentials conflict

**Resolution Steps:**
1. Confirm Caps Lock is off and correct username format is used (e.g., user@domain.com vs. DOMAIN\user)
2. Check Active Directory (if applicable) — verify account is not locked, disabled, or expired
3. Unlock account in AD if locked: `Active Directory Users and Computers > Find User > Unlock Account`
4. Reset password and force change at next login
5. For Microsoft 365: reset via Microsoft Admin Center or self-service password reset (SSPR) if enabled
6. Clear cached credentials: `Control Panel > Credential Manager > Remove stale entries`
7. Have user log in fresh and confirm access

**Escalate If:**
- Account shows no lockout in AD but user still cannot log in
- MFA device is lost or inaccessible
- Domain trust relationship errors appear

---

## 3. Printer Not Found or Won't Print

**Symptoms:**
- Printer not showing in available devices
- Print jobs stuck in queue
- "Driver unavailable" error

**Likely Causes:**
- Printer offline or powered off
- Print spooler service stopped
- Incorrect or corrupt driver
- Network printer IP changed

**Resolution Steps:**
1. Confirm printer is powered on and shows "Ready" on its display
2. For network printers: ping the printer IP to confirm it's reachable (`ping 192.168.x.x`)
3. Restart the Print Spooler service: `Services.msc > Print Spooler > Restart`
4. Clear stuck print queue: Stop Spooler > delete files in `C:\Windows\System32\spool\PRINTERS` > restart Spooler
5. Remove and re-add the printer via `Settings > Printers & Scanners > Add a Printer`
6. Download and reinstall the latest driver from the manufacturer's website
7. For network printers, confirm IP has not changed — update the port if needed

**Escalate If:**
- Driver install requires elevated permissions not available at Tier 1
- Printer is shared from a print server and server-side changes are needed

---

## 4. Computer Running Slowly

**Symptoms:**
- Long boot times
- Programs freeze or take long to open
- High CPU or memory usage

**Likely Causes:**
- Too many startup programs
- Low disk space
- Malware or unwanted background processes
- Insufficient RAM for workload
- Failing hard drive

**Resolution Steps:**
1. Check Task Manager (`Ctrl + Shift + Esc`) — identify high CPU/RAM processes
2. Disable unnecessary startup programs: `Task Manager > Startup tab`
3. Run Disk Cleanup: `cleanmgr` in Run dialog
4. Check disk space — flag if C: drive is below 10% free
5. Run a full malware scan using Windows Defender or approved endpoint tool
6. Check hard drive health: `wmic diskdrive get status` or use CrystalDiskInfo
7. Restart device and retest — document improvement or lack thereof

**Escalate If:**
- Disk health shows warnings or failures
- Malware detected that cannot be removed at Tier 1
- RAM upgrade or hardware replacement is needed

---

## 5. Microsoft 365 / Outlook Not Working

**Symptoms:**
- Outlook won't open or crashes on launch
- Emails not sending or receiving
- "Need Password" prompt loops without accepting credentials
- Calendar not syncing

**Likely Causes:**
- Corrupt Outlook profile
- Expired or invalid Microsoft 365 license
- Cached credentials conflict
- Connectivity issue with Microsoft servers

**Resolution Steps:**
1. Check Microsoft 365 service health at `admin.microsoft.com` — confirm no outage
2. Sign out and back in to Microsoft 365 via browser at `office.com`
3. Clear cached credentials in Credential Manager — remove all Microsoft/Office entries
4. Repair Outlook profile: `Control Panel > Mail > Show Profiles > Repair`
5. If repair fails, create a new Outlook profile
6. Run the Microsoft Support and Recovery Assistant (SaRA tool) for automated diagnosis
7. Confirm license is assigned in Microsoft Admin Center

**Escalate If:**
- License is expired or unassigned — requires admin action
- Issue is tenant-wide or service outage confirmed
- Mailbox corruption suspected

---

## 6. Blue Screen of Death (BSOD)

**Symptoms:**
- System crashes with a blue screen and stop code
- Device restarts unexpectedly
- Common stop codes: IRQL_NOT_LESS_OR_EQUAL, MEMORY_MANAGEMENT, SYSTEM_THREAD_EXCEPTION_NOT_HANDLED

**Likely Causes:**
- Faulty or outdated drivers
- Failing RAM or hard drive
- Recent Windows update conflict
- Overheating

**Resolution Steps:**
1. Note or photograph the stop code displayed
2. Boot into Safe Mode and check stability — if stable, issue is driver or software related
3. Check Event Viewer (`eventvwr.msc`) > Windows Logs > System for critical errors around crash time
4. Run `sfc /scannow` in elevated Command Prompt to check system file integrity
5. Run `chkdsk /f /r` to check disk for errors
6. Uninstall any recently installed drivers or updates that preceded the BSOD
7. Run Windows Memory Diagnostic (`mdsched.exe`) to test RAM

**Escalate If:**
- BSODs are recurring after all steps
- Hardware failure is suspected (RAM, SSD, GPU)
- Data recovery is needed

---

## 7. User Cannot Access a Shared Drive or Network Folder

**Symptoms:**
- "Access Denied" when opening a mapped drive
- Drive letter missing or shows as disconnected
- Path not found error

**Likely Causes:**
- Permissions not assigned in Active Directory
- Network connectivity issue
- Drive mapping script not running at login
- Group Policy not applied correctly

**Resolution Steps:**
1. Confirm user is connected to the network or VPN (if remote)
2. Try accessing the share directly via UNC path: `\\servername\sharename`
3. Verify the user's AD group membership includes the group with share permissions
4. Remap the drive manually: `net use Z: \\servername\sharename /persistent:yes`
5. Run `gpupdate /force` to reapply Group Policy
6. Check if other users in the same role/department can access the share — isolates permissions vs. infrastructure

**Escalate If:**
- Permissions changes are needed in AD or on the file server
- Server-side share or NTFS permissions need to be reviewed
- VPN or domain connectivity issues are suspected at infrastructure level

---

## Escalation Reference

| Tier | Handles |
|------|---------|
| Tier 1 (This Runbook) | Password resets, basic connectivity, printer issues, software reinstalls, account unlocks |
| Tier 2 | Active Directory changes, Group Policy, server-side issues, VPN configuration |
| Tier 3 | Infrastructure, network architecture, security incidents, data recovery |

---

## Notes
- Always document every step taken in the ticketing system before escalating
- Confirm resolution with the end user before closing a ticket
- If unsure, escalate early — it is better to loop in Tier 2 than to make the problem worse
