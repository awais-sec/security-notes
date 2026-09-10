# Incident Response and Management: Digital Forensic Artifacts and Deep-Dive Investigations

## 1. The Forensic Investigator's Checklist

When a system is compromised, a forensic investigator must prove specific facts to reconstruct the "attack story":

- **Timing** — when the system was first compromised
- **Impacted Users** — which specific user accounts were affected
- **Entry Method** — how the attacker gained access (e.g. checking VPN/RDP logs or email attachments)
- **Tools & Commands** — which files, scripts, or manual commands were used by the attacker
- **Exfiltration Evidence** — whether sensitive files were accessed, zipped, or transferred out
- **Final Strike** — when the ransomware or a persistent backdoor was finally deployed

## 2. Web Browser Artifacts

- **History** — stored in `WebCacheV01.dat` (Edge/IE — ESE database) or `places.sqlite` (Firefox — SQLite)
- **Cookies** — track user sessions and website visits
- **Cache** — locally saved web components (images, scripts) for faster loading
- **Tools** — ESEDatabaseView, DB Browser for SQLite, ChromeCacheView

## 3. Windows Registry Hives

- **Core Hives (System-wide)** — SAM, SYSTEM, SOFTWARE (location: `\System32\config`)
- **User Hives** — `NTUSER.DAT` (user profile) and `USRCLASS.DAT` (AppData)

**Execution Evidence:**
- **UserAssist** — GUI programs run by a specific user (count and last run time)
- **ShimCache** — programs executed (paths and last modification dates)
- **Amcache** — executed programs metadata (includes SHA1 hashes for identification)

**Recent Activity:** MRU (Most Recently Used files), RecentDocs, and Shellbags (recently accessed folders/network shares).

**Persistence:** Run/RunOnce registry keys showing if malware was set to start automatically.

**External Devices:** USBSTOR keys indicating if a malicious USB was connected.

## 4. Windows Event Logs (.evtx)

**Location:** `C:\Windows\System32\winevt\Logs`

**Security Log (Critical IDs):**
- `4624` — Successful Logon (Type 10 = RDP)
- `4625` — Failed Logon (identifies brute-force)
- `4720`/`4732` — Account creation / Local group member added
- `4672` — High-privilege (admin) logon

**System Log:**
- `7045` — New Service Installed (classic sign of PsExec or persistence)
- `7040` — Service start type changed

**Other Logs:**
- **PowerShell** (`400`/`4104`) — script execution and content
- **TerminalServices** — ID `21` (Logon success) and ID `25` (Reconnection success)
- **Defender** (`1116`/`1117`) — malware detection and action taken

## 5. Additional Log Sources

- **Firewall/Proxy** — shows network connections to C2 servers
- **VPN Logs** — reveal initial access points and GeoIP anomalies (e.g. unexpected logins from Russia)
- **PowerShell History** — text files located in `%APPDATA%` showing exact commands typed by attackers
- **Memory Artifacts (RAM)** — vital for detecting malware that stays in memory rather than on disk; can reveal active network connections, injected code, and traces of stolen credentials
- **Sysmon Logs** — supplement Windows Event Logs with detailed process, network, and file activity

## 6. Deep-Dive: Investigating RDP Brute Force

**The Vector:** over 50% of successful attacks exploit publicly exposed RDP servers.

**Key Source:** `Security.evtx`

**Essential Event IDs:**
- `4625` — failed logon (high counts indicate brute-force)
- `4624` (Logon Type 10) — successful Remote Desktop logon

**Case Evidence:** finding successful Type 10 logons from a suspicious foreign IP (e.g. Russia) using a common name like "administrator."

**Tools Used:** KAPE for log collection and EvtxExplorer to parse logs into readable CSVs.

## 7. Deep-Dive: Investigating Phishing (Precursors)

**The Vector:** Email → weaponized document (`.docm`) → macros → PowerShell.

**Memory Forensic Artifacts (Volatility 3):**
- `malfind` — detects code injection (common in `rundll32.exe`)
- `pstree` — reveals suspicious parent-child process relationships
- `netscan` — identifies malicious Command & Control (C2) IP connections

**Disk & Registry Artifacts:**
- **Prefetch** — shows `winword.exe` accessing suspicious files in Outlook temp folders
- **Registry (Run Key)** — reveals persistence (e.g. `rundll32.exe` launching a random-named file like `jwkgphpq.euz`)
- **PowerShell Logs** — captures the exact script used to download the payload from remote URLs

## 8. Deep-Dive: Post-Exploitation

### Credential Access
- **Hacking Tools** — Mimikatz and its variants (SafetyKatz, Invoke-Mimikatz) are the "gold standard" for dumping passwords from the LSASS process
- **Evidence of Execution** — if the tool is deleted, check Amcache (metadata/hashes), Shimcache, or UserAssist
- **Built-in Abuse** — attackers use `rundll32.exe` to call `MiniDump` in `comsvcs.dll` to dump LSASS memory without using custom hacking tools
- **Kerberoasting** — using tools like `Rubeus.exe` to request service tickets and crack passwords offline

### Reconnaissance
- **Network Scanning** — legitimate tools like SoftPerfect Network Scanner (`netscan.exe`) are often dropped in `C:\Users\Public`
- **AD Recon** — `AdFind.exe` is extremely common; look for batch files (e.g. `a.bat`) that automate queries for domain trusts, users, and computers

### Lateral Movement
- **Administrative Shares** — abuse of `C$`, `ADMIN$`, and `IPC$` to copy files or browse remote hosts
- **PsExec** — a classic sign of lateral movement is Event ID `7045` (New Service Installed) in the System log showing a service named `PSEXESVC`
- **Enabling RDP** — attackers often use scripts (e.g. `rdp.bat`) to modify the registry (`fDenyTSConnections = 0`) and firewall rules to force-enable Remote Desktop on target hosts

## 9. Deep-Dive: Data Exfiltration

### Web Browser Abuse
- **Goal** — uploading data to file-sharing sites (e.g. DropMeFiles)
- **Microsoft Edge** — `WebCacheV01.dat` (ESE DB); look at `Container_7` for URLs and Webkit-format timestamps
- **Mozilla Firefox** — `places.sqlite` (SQLite DB); check the `moz_places` table for visited URLs
- **Pivot Point** — Bing/Google searches for archiving tools like 7-Zip often precede exfiltration

### Cloud Service Clients
- **Installation Evidence** — check the SOFTWARE registry hive (`...\CurrentVersion\Uninstall`) for tools like MEGAsync
- **Rich Evidence** — `MEGAsync.log` (in AppData) reveals the exact upload queue, file paths, and the attacker's account name

### Masquerading & Sync Tools
- **Rclone** — a common command-line tool for cloud transfers; attackers often rename `rclone.exe` to `svchost.exe` to hide
- **Detection** — legitimate `svchost.exe` lives in `\System32`; malicious versions are often found in `\Windows` or `\Users\Public`
- **Config Files** — look for `rclone.conf` to find configured cloud accounts

### Custom Exfiltration Tools
- **StealBit** — custom tool used by LockBit 2.0
- **ExMatter** — used by BlackMatter
- **Sidoh** — used by Ryuk
- **Self-Deletion** — tools like StealBit may use a `-delete` flag that executes a ping delay followed by `fsutil` to wipe itself

## 10. Deep-Dive: Ransomware Deployment

### Via RDP (Manual)
**Method:** attackers copy the ransomware executable to the host and run it manually via an RDP session.

**Forensic Evidence:**
- **MFT Analysis** — identifies the exact time encryption started by looking for the creation of ransom notes (e.g. `how_to_decrypt.hta`)
- **UserAssist** — shows the specific user account used to manually launch the malware
- **Recon Tools** — look for evidence of `NS.exe` (mounts network shares) or `Everything.exe` (searches for large data files)
- **Event Logs** — ID `21`/`25` in TerminalServices logs, or ID `4624` Type 10 in Security logs, show the source IP of the deployment session

### Via Administrative Shares (Automated)
**Method:** abuse of `C$` or `ADMIN$` to push the malware, often using PsExec or custom batch files to run it enterprise-wide.

**Forensic Evidence:**
- **System Log (ID 7045)** — a new service installation pointing to a suspicious file on a network share
- **Security Log (ID 4672/4624)** — high-privilege logon activity by the Administrator account at the time of deployment

### Via Group Policy (GPO)
**Method:** ransomware like LockBit has built-in features to modify GPOs on a Domain Controller to force-distribute and execute itself on every host.

**Artifacts (on Domain Controller):**
- **SYSVOL Scripts** — malicious `.exe` files found in `\SYSVOL\domain\scripts`
- **GPO XML Files** — `ScheduledTasks.xml` (creates tasks to run ransomware) and `Files.xml` (copies the malware to user desktops)
- **PowerShell Logs** — records of commands used to force immediate Group Policy updates across the network

## 11. Targeting Backups (Deployment Stage)

- **Target #1: Backups** — actors delete backups first; they often disable the Volume Shadow Copy Service (VSS)
- **Disabling Defense** — use Group Policy (GPO) scripts or the security console to turn off antivirus
- **Execution** — automated via GPO or PsExec
- **Exclusions** — ransomware avoids encrypting system folders (e.g. `\Windows`) so the computer stays usable enough to pay the ransom
- **Final Pressure** — some strains (like REvil) use DDoS attacks if the victim won't negotiate

---
*Source: coursework notes (AI-compiled study notes), Incident Response and Management module — covers the forensic artifact and investigative deep-dive content unique to this module's lecture series.*
