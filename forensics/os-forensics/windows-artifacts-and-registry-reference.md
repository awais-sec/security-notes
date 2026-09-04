# Windows Artifacts, Commands, and Registry Reference

## Commands

| Command | Purpose |
|---|---|
| `date /t` & `time /t` | Displays current date and time |
| `psloggedon [-] [-l] [-x] [\\computername \| username]` | Displays users logged on locally and via resources |
| `net sessions` | Information about local computer sessions |
| `net file [ID [/close]]` | Details of open shared files on a server |
| `psfile [\\RemoteComputer [-u Username [-p Password]]] [Id \| path] [-c]` | Shows remotely opened files on a system |
| `adplus.vbs` | Script that dumps process memory |
| `promiscdetect` | Checks if network adapters are running in promiscuous mode |
| `promqry` | Detects network interfaces running in promiscuous mode |
| `doskey /history` | Displays previously typed commands |
| `dir /o:d` | Displays the time and date of OS installation |
| `fsutil` | Run elevated to query/enable/disable "Last Access Time" |
| `reg.exe` | Collects information from specific registry keys and values |
| `wevtutil el` | Displays a list of available Event Logs |
| `wevtutil gl` | Lists configuration information about a specific Event Log |
| `openfiles` | Query, display, or disconnect opened files/directories (`/disconnect`, `/query`, `/local`) |
| `tasklist` | Lists applications and their PID for all running tasks |
| `pslist` | Elementary information about all running processes |
| `pslist -x` | Processes, memory information, and threads |
| `listdlls` | Lists all DLLs loaded in all processes |
| `handle` | Info about open handles: ports, registry keys, sync primitives, threads, processes |
| `netstat` | Network connections, ports used by a process, and connected protocol |
| `ipconfig` | Info about NICs and their status |
| `nbtstat` | Views the NetBIOS name table cache |

### Additional tools worth knowing

- **Autoruns (Sysinternals)** and **Silent Runners**: For examining what's set to auto-start on a system.
- **`wmic path win32_USB`**: Used to trace out USB devices from a suspect machine.
- **Bulk Extractor**: Useful for building a memory dump of a system.

## File Paths

| Path | Contents |
|---|---|
| `C:\Windows\System32\spool\PRINTERS` | Default path for print spool files (`.SPL`, `.SHD`) |
| `C:\Users\AppData\Local\Microsoft\Windows\Explorer` | Thumbcache files (Windows 10 and 7) |
| `%SystemRoot%\Memory.dmp` | Default memory dump file path |
| `%SystemRoot%\Minidump` | Small memory dump file list |
| `C:\Users\{user}\AppData\Roaming\Mozilla\Firefox\Profiles\XXXXXXX.default\places.sqlite` | Firefox history |
| `C:\Users\{user}\AppData\Local\Google\Chrome\User Data\Default\Cache` | Chrome cache |
| `C:\Users\{user}\AppData\Local\Google\Chrome\User Data\Default` | Chrome history and cookies |
| `C:\Users\Admin\AppData\Local\Microsoft\Windows\WebCache` | Edge cache |
| `C:\Users\Admin\AppData\Local\Packages\Microsoft.MicrosoftEdge_8wekyb3d8bbwe\AC\MicrosoftEdge\Cookies` | Edge cookies |
| `C:\Users\Admin\AppData\Local\Microsoft\Windows\History` | Edge history |
| `\System Volume Information\ - restore {GUID}\RP##` | Restore points |
| `\System Volume Information\ - restore {GUID}\RP##\rp.log` | Restore point log file |
| `\System Volume Information\ - restore {GUID}\RP##\change.log` | Change log for restore points |
| `C:\Windows\System32\winevt\Logs` | Windows Event logs |
| `C:\Users\<Username>\AppData\Local\Mozilla\Firefox\Profiles\XXXXXXX.default\cache2` | Firefox cache |
| `C:\Users\<Username>\AppData\Roaming\Mozilla\Firefox\Profiles\XXXXXXX.default\cookies.sqlite` | Firefox cookies |
| `...\AC\MicrosoftEdge\User\Default\DataStore\Data\nouser1\xxxxx\DBStore\spartan.edb` | ESE database for Edge |
| `...\AC#1001\MicrosoftEdge\Cache` | Edge cached files |
| `...\AC\MicrosoftEdge\User\Default\Recovery\Active` | Edge last active browsing session data |
| `...\WebCache\WebCacheV01.dat` | Edge history, cookies, HTTP POST headers, downloads |
| `...\AC\MicrosoftEdge\User\Default\Recovery\Active\{browsing-session-ID}.dat` | Edge private-mode session records |
| `C:\Windows\System32\config` | System information (GUI path) |

## Registry Paths

| Key | Purpose |
|---|---|
| `HKLM\SYSTEM\CurrentControlSet\Control\ComputerName\ActiveComputerName` | Computer name |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList` | Microsoft Security IDs |
| `HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate` | Disable last-access-time updates |
| `HKLM\SYSTEM\CurrentControlSet\Services\EventLog` | Event Log configuration |
| `HKLM\System\ControlSet00x\Services\EventLog` | Specific Event Log config (Windows 10) |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs` | MRU list |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU` | Another MRU list |
| `HKCU\Software\Microsoft\Internet Explorer\TypedURLs` | TypedURLs MRU list |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePidlMRU` | MRU for Open/SaveAs dialogs |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Map Network Drive MRU` | Map Network Drive Wizard MRU |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2` | Volumes added via Map Network Drive or `net use` |
| `HKLM\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\SystemRestore` | Restore point settings |
| `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters` | Prefetch control |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\EventLog` | Event log configuration |
| `HKLM\SYSTEM\CurrentControlSet\Services\EventLog\<Log>` | Specific event log configuration |
| `HKEY_CLASSES_ROOT\exefile\shell\open\command` | Command associated with exefile shell open |
| `HKLM\SYSTEM\CurrentControlSet\Services\LanmanServer\Shares` | Shared resources |
| `HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation` | Time zone settings |
| `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles\{GUID}` | Connected wireless SSIDs |
| `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR` | Connected USB devices |
| `HKLM\SYSTEM\CurrentControlSet\Control\DeviceClasses` | Specific device classes |
| `HKCU\System` | Registry keys tracking user activity |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count` | UserAssist keys |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` | Startup location |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce` | Startup location |
| `HKLM\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\Winlogon` | Startup location |
| `HKLM\SOFTWARE\Microsoft\Active Setup\Installed Components` | Startup location |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\ShellServiceObjectDelayLoad` | Startup location |
| `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager` | Startup location |
| `HKLM\SYSTEM\CurrentControlSet\Services` | Startup location |
| `HKLM\SYSTEM\CurrentControlSet\Services\WinSock2\Parameters\Protocol_Catalog9\Catalog_Entries` | Startup location |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` | Startup location |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce` | Startup location |
| `HKCU\Control Panel\Desktop` | Startup location |
| `HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows` | Startup location |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders` | User startup folder settings |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders` | User startup folder settings |
| `HKLM\SYSTEM\CurrentControlSet\Control\ShutdownTime` | Last system shutdown time |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion` | ProductName, CurrentBuildNumber, CSDVersion (also OS version / NTFS type) |
| `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management` | Page file path |

## Quick Reference Notes

- If the prefetch file is deleted and Event Log/software file aren't available either, **Amcache.hive** is a fallback source for the same kind of execution evidence.
- `spool file`: relates to the printer (see `C:\Windows\System32\spool\PRINTERS` above).
- RAM and ROM info generally falls under system information collection alongside the paths above.
