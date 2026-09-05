# Assignment #1 — NtfsDisableLastAccessUpdate Key in Windows 10 and 11

*Advanced Forensics, presented to Kaukab Jamal Zuberi. Submitted by Awais Ahmed, BS-DFCS, Sp-2023, Sec-A, Roll No. 008.*

In Windows 10 and Windows 11, the `NtfsDisableLastAccessUpdate` registry key controls whether the NTFS file system updates the "Last Access Time" attribute for files and directories. By default, this key is set to `0x80000003`, which corresponds to "System Managed," and by default disables these updates to enhance performance.

In "System Managed" mode, the NTFS driver decides whether to enable or disable Last Access Time updates during system boot. By default, updates are enabled when the system volume size is 128 GiB or less, and disabled if the volume size exceeds this threshold. This behavior is influenced by the `NtfsLastAccessUpdatePolicyVolumeSizeThreshold` registry value, which specifies the volume size limit in GiB. However, this registry value does not exist by default.

The default setting aims to improve system performance by reducing the overhead caused by frequent updates to the Last Access Time attribute. However, disabling these updates can affect certain applications, such as backup and remote storage programs, that rely on this timestamp.

Users can modify this behavior by changing the `NtfsDisableLastAccessUpdate` registry key to one of the following values:

| Value | Meaning |
|---|---|
| `0x80000000` | User Managed, Last Access Updates Enabled |
| `0x80000001` | User Managed, Last Access Updates Disabled |
| `0x80000002` | System Managed, Last Access Updates Enabled |
| `0x80000003` | System Managed, Last Access Updates Disabled (default) |

## References

1. [fsutil behavior — Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/fsutil-behavior)
2. [The Last Access updates are (almost) back — dfir.ru](https://dfir.ru/2018/12/08/the-last-access-updates-are-almost-back/)
