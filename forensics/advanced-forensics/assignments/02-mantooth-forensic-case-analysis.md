# Assignment #2 — Mantooth Forensic Case Analysis

*Advanced Forensics, presented to Kaukab Jamal Zuberi. Submitted by Awais Ahmed, BS-DFCS, Sp-2023, Roll No. 008.*

1. **Computer Name**: identified system name `WES-MANTOOTH-WORKSTATION`.
2. **Time Zone**: configured region MST (UTC-7).
3. **IDE Hard Drive Identification**: Maxtor 6E020L0 (firmware NAR61EA0).
4. **Network Configuration**: assigned IPv4 `192.168.1.106` (leased from DHCP server `192.168.1.1` on 14 July 2007, 07:40:19 MST).
5. **Prefetch Configuration**: EnablePrefetcher setting 3 (full optimization: boot and application prefetching enabled).
6. **TrueCrypt Service Configuration**: service initialization set to system-level startup (`0x1`).
7. **Last System Shutdown**: recorded shutdown time 4 October 2007, 21:27:45 MST.
8. **Canon PowerShot SD500 Device**: serial identifier `5&1ec84238&0&4` (connected 14 July 2007, 22:56:41 PKT).
9. **Connected Portable Devices**:
   1. Apple iPod (serial `000A270014B302AB&0`)
   2. SanDisk Cruzer Mini (serial `SNDK3066A40516400406&0`)
   3. Sony DSC Camera (serial `6&382957cd&0`)
   4. TREK TD2SMART_G3M (serial `10120515511949&0`)
   5. Generic USB 2.0 Flash (serial `6&2507d51a&0&AA10000000000623&0`)
10. **Default Browser**: Microsoft Internet Explorer.
11. **Network Interface Cards**: Realtek RTL8139/810x Family Fast Ethernet NIC (wired); a wireless adapter inferred from network profiles but not explicitly named.
12. **Wi-Fi Network Profile**: `Frankenwave2`.
13. **Removable Media Connections**: Super Flash 1GB (ID `0000000000C80F`), IntelliMouse Optical (ID `5&40df8c9&0&1`), SanDisk Cruzer Mini (ID `20043513310C7A22D0C8`).
14. **Installed Software Timestamps**:
    1. ActiveTouchMeetingClient — 10 October 2007, 10:12:40
    2. TrueCrypt — 11 April 2007, 01:37:31
    3. WinRAR — 27 February 2007, 20:01:25
    4. QuickTime — 13 March 2007, 23:36:51
    5. Trillian — 27 February 2007, 19:39:25
15. **OS Registration Metadata**: registered owner Wes Mantooth; organization Volturi Enterprises; install date 20 August 1972 (anomalous, likely tampered).
16. **User Profiles**: Wes Mantooth (Admin), Dracula (Limited).
17. **SAM File User Activity**:
    - Wes Mantooth: last login 12 February 2008; password hint "in your face"; 96 logins, with a recent failed attempt on 12 February 2008.
    - Dracula: limited account, last active 2 April 2007.
18. **Primary Investigative Focus**: Wes Mantooth's account, due to administrative privileges, frequent activity, and proximity to evidence timelines.
19. **Recent PDF Access**: `order851797-2007-04-12-13-17-02.pdf` (opened via default viewer).
20. **Paint-App Edited Files**: `nationaltall.bmp`, `prescription.gif`, `nationaltal.gif`.
21. **Frequently Launched Applications**: Remote Desktop (`mstsc`), Disk Utility (`gdisk32.exe`), Registry Editor (`regedt32`).
22. **ATM_THEFTS1.ppt Location**: `E:\Business Ideas\ATM_THEFTS1.ppt`.
23. **Investigative Actions for the ATM File**: conduct temporal metadata analysis, recover historical versions, and assess document content for illicit planning.
24. **Internet Explorer Homepage**: default start URL `http://www.google.com/`.
25. **IE Download Directory**: `C:\Wes Mantooth\Desktop\Latest True Crypt`.
26. **Browser Bookmarks**: Yahoo, MSN, Windows Live, Microsoft, and a generic "Links" folder.
27. **Media Player Activity (F: Drive)**: played audio files `F:\Sounds and Videos\wizoz18d.wav`, `pf3.wav`.
28. **Media File Analysis Steps**: trace file origins, correlate with external storage logs, and verify timestamps against system events.
29. **Recent PowerPoint Usage**: `ATM_THEFTS1.ppt` (paths `E:\Business Ideas\` and `C:\Desktop`).
30. **Critical Documents for Review**:
    1. `ATM_THEFTS1.ppt.lnk` — the file name suggests it's related to ATM theft, so it may contain something useful.
    2. `Checks.lnk` — may contain information related to fake cheques, since the accused reportedly used to print fake cheques.
    3. `NTFS Junction Guide.mht.lnk` — the name suggests instructions on manipulating NTFS junctions, which could show which junction points the accused manipulated and help establish evidence.
    4. `Media Cleanup Folder.lnk` — may contain items the accused was trying to hide by clearing them.
    5. `Secret Files.lnk` — may contain something suspicious or case-relevant, given the name.
31. **Run Command Executables**: `cmd`, `notepad`, `defrag`, `regedt32`, `msconfig`.
32. **Manually Typed URLs**: TigerDirect, Newegg, Gmail, Yahoo, Tucows.
33. **Email Addresses Associated**: `wes_mantooth@mail.goog`, `mantooth2007@aol.com`, `mantooth@google.com`.
34. **WinVNC Credentials**: registry-stored DES hash `A8 52 08 92 12 40 45 2D`, decrypted to "password".
35. **Yahoo Messenger ID**: `wes_mantooth@yahoo.com`.
36. **Default Printing Device**: Epson RX420 Photo Printer (used for unauthorized cheque replication).
