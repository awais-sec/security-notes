# Windows Registry Forensic Artifact Paths

Reference list of Windows Registry paths (and a few related file-system locations) relevant to forensic artifact recovery, grouped by common base path.

## HKLM\SYSTEM\ControlSet001

- **Computer name**: `HKLM\SYSTEM\ControlSet001\Control\ComputerName\ComputerName`
- **Last shutdown time**: `HKLM\SYSTEM\ControlSet001\Control\Windows`

## HKLM\SYSTEM\CurrentControlSet\Control

- **Time zone name**: `HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation\TimeZoneKeyName`
- **Network cards used**: `HKLM\SYSTEM\CurrentControlSet\Control\Class\{4D36E972-E325-11CE-BFC1-08002BE10318}`
- **Printers info**: `HKLM\SYSTEM\CurrentControlSet\Control\Print\Printers`

## HKLM\SYSTEM\CurrentControlSet\Enum

- **IDE hard drive name**: `HKLM\SYSTEM\CurrentControlSet\Enum\IDE`
- **USB devices (removable/portable)**: `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR`

## HKLM\SYSTEM\CurrentControlSet\Services

- **TrueCrypt service start type**: `HKLM\SYSTEM\CurrentControlSet\Services\truecrypt`
- **IP address, DHCP info**: `HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces`

## HKLM\SYSTEM\CurrentControlSet\Control\Session Manager

- **Prefetch setting**: `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters\EnablePrefetcher`

## HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer

- **Recent PDFs and MRU apps**: `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`
- **Programs opened via Run**: `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU`
- **Recent files via Paint**: `HKCU\Software\Microsoft\Windows\CurrentVersion\Applets\Paint`
- **Removable drive letters**: `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2`

## HKCU\Software\Microsoft\Internet Explorer

- **IE start page**: `HKCU\Software\Microsoft\Internet Explorer\Main\Start Page`
- **IE download directory**: `HKCU\Software\Microsoft\Internet Explorer`
- **Typed URLs in IE**: `HKCU\Software\Microsoft\Internet Explorer\TypedURLs`

## HKCU\Software\Microsoft\Windows NT\CurrentVersion

- **Wi-Fi profile names**: `HKCU\Software\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles`

## HKCU\Software\Microsoft\Windows\Shell\Associations

- **Default web browser**: `HKCU\Software\Microsoft\Windows\Shell\Associations\UrlAssociations\http\UserChoice`

## HKCU\Software\Microsoft\Office[version]\PowerPoint

- **Recent PowerPoint files**: `HKCU\Software\Microsoft\Office\[version]\PowerPoint\Recent File List`

## HKCU\Software\Yahoo\pager

- **Yahoo Messenger username**: `HKCU\Software\Yahoo\pager\Yahoo! User ID`

## HKCU\Software\ORL\WinVNC3

- **VNC password (encrypted)**: `HKCU\Software\ORL\WinVNC3`

## HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion

- **Installed software + timestamps**: `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`

## HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion

- **OS info (organization, owner, install date, product ID)**: `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion`

## HKLM\SYSTEM\MountedDevices

- **Mounted drives info**: `HKLM\SYSTEM\MountedDevices`

## HKLM\SAM\SAM\Domains\Account\Users

- **User profiles with details (username, created, last login, etc.)**: `HKLM\SAM\SAM\Domains\Account\Users`
- **List all user profiles (names)**: `HKLM\SAM\SAM\Domains\Account\Users\Names`

## File System Locations

- **User accounts, folder names**: `C:\Documents and Settings\`
- **Favorites links in IE**: `C:\Documents and Settings\[Username]\Favorites`

## Using Autopsy (Not Registry Paths)

- **File path for `ATM_THEFTS1.ppt`**: use Keyword Search or the Recent Documents view.
- **Files opened by Media Player from the F:\ drive**: use Extracted Content → Recent Files → Media Player, or search the F:\ drive using keywords (`.mp3`, `.avi`, etc.).
