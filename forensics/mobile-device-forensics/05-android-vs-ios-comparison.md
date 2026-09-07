# Android vs. iOS: Comparative Forensics Reference

## Part 1: OS Foundations and Evolution

| Feature | Android | iOS |
|---|---|---|
| Development | Open Handset Alliance (Google-led); Linux-based | Proprietary; derived from core OS X technologies |
| License | Apache License; allows manufacturer modification (Samsung, Sony, etc.) | Closed source; unified across iPhone, iPad, Apple Watch, and Apple TV |
| Naming | Confectionery-based (alphabetical) from 1.5 to 9.0; switched to numerical ordering with Android 10 | Originally "iPhone OS"; renamed to iOS to reflect multi-device support |
| Version sync | Major updates typically occur once per year | Hardware and software are co-developed for specific feature support |

### Software Architecture Layers

**Android stack**

- **Linux Kernel**: base layer; manages process/memory, security, networking, and drivers (USB, Wi-Fi, audio).
- **HAL (Hardware Abstraction Layer)**: library modules that implement interfaces for specific hardware, allowing vendors to modify features without changing high-level systems.
- **Native C/C++ Libraries**: includes SQLite, WebKit, Media Framework, and SSL.
- **Android Runtime (ART)**: executes apps; replaced Dalvik in version 5.0.
- **Java API Framework**: provides managers like Telephony (voice calls), Location, Package, Notification, and Window.
- **System Apps**: user interface layer (Dialer, Email, etc.).

**iOS stack**

- **Cocoa Touch**: frameworks for visual interface, touch-based input, and multitasking.
- **Media**: graphics, audio, and video technologies.
- **Core Services**: fundamental services like iCloud, location, and social media.
- **Core OS**: base layer sitting on hardware; handles low-level networking (BSD sockets), memory management, and threading (POSIX threads).

### Runtime Environments

- **Dalvik** (Android <5.0): used Just-In-Time (JIT) compilation; converted `.class` files into a single `.dex` file using the `dx` tool.
- **ART** (Android 5.0+): uses Ahead-Of-Time (AOT) compilation during app installation; uses the `dex2oat` utility to generate ELF executables from `.dex` files, reducing power consumption and increasing efficiency.
- **iOS execution**: applications don't interact with hardware directly but through a system interface that abstracts hardware changes.

### Security Architecture

| Security Feature | Android | iOS |
|---|---|---|
| Access Control | SELinux (since 4.3); uses Mandatory Access Control (MAC) to isolate apps. Operates in Permissive or Enforcing modes | Principle of Least Privilege (PoLP); two user roles: root and mobile |
| Sandboxing | Each app assigned a unique UID/GID; kernel prevents one app from reading another's data | Places apps in restricted areas; limits access to files, network, and hardware |
| Boot Security | Verified Boot (since 7.x); each stage validates the next. Sideloading other OSes is prevented | Secure Boot Chain: Boot ROM (Secure ROM) verifies LLB, which verifies iBoot, which verifies the kernel |
| Biometrics | Supports various hardware options; face-lock and fingerprint-lock | Face ID (1 in 1,000,000 unlock chance) vs. Touch ID (1 in 50,000 chance) |
| Hardware Key | TEE (Trusted Execution Environment) on a separate microprocessor for secret data | Secure Enclave, Secure Element, and Crypto Engine |

### Encryption Standards

**Android transition**

- **FDE (Full Disk Encryption)**: mandated in 6.0; encrypts user data using a key protected by device PIN/pattern. Works with eMMC block devices.
- **FBE (File-Based Encryption)**: introduced in 7.0; different files use different keys. Allows Direct Boot features (notifications/alarms) without full decryption.

**iOS transition**

- Filesystem encryption since iPhone 4.
- **Data Protection**: links passcode to hardware encryption to protect data at rest.
- **Wiping**: "Erase All Content" removes the encryption keys, making data recovery impossible.

**A way to think about it**: Android's layered security resembles a large organization with multiple department heads (the API Framework managers) operating on a relatively open floor plan (open source), with a security guard (SELinux) who can either just take notes (Permissive mode) or physically block access (Enforcing mode). iOS security is closer to a single tightly run facility: every entry point is checked by the same fixed chain of verification (the Secure Boot Chain), and if the key is lost (the encryption key), there's no spare — the lock is simply destroyed (Data Wiping), making the space permanently inaccessible.

## Part 2: Filesystems, Acquisition Methodologies, and Artifact Locations

### Filesystems and Partitioning

| Feature | Android | iOS |
|---|---|---|
| Primary Filesystem | EXT4 (standard since Gingerbread); YAFFS2 (older NAND); F2FS (Samsung optimization) | APFS (iOS 10.3+); legacy was HFSX (case-sensitive) |
| APFS Features | N/A | Supports Clones (instant copies), Snapshots (point-in-time instances), and Space Sharing |
| Disk Partitions | Typically 6+ partitions: `/boot`, `/system`, `/recovery`, `/data`, `/cache`, `/misc` | Two logical partitions: System Partition (read-only OS) and User Data Partition (mounted at `/private/var`) |
| Pseudo Filesystems | Uses `proc` (kernel data), `sysfs` (configuration), `devpts` (terminals), and `tmpfs` (RAM) | N/A |

### Forensic Acquisition Methodologies

**Logical Acquisition**

- *Android*:
  - ADB Pull: retrieves files directly; requires root to access `/data/data`.
  - ADB Backup: creates `.ab` files; does not require root but depends on app developer permissions.
  - AFLogical: uses Content Providers to extract SMS, contacts, and call logs to an SD card in CSV format; works on non-rooted devices.
- *iOS*:
  - iTunes Backups: standard sync protocol; encrypted backups are preferred as they contain more data (passwords, Wi-Fi, browsing history).
  - iCloud Backups: online retrieval; requires Apple ID/password or extracted tokens.

**Physical & Filesystem Acquisition**

- *Android*:
  - `dd` command: creates bit-by-bit images of specific blocks (e.g., `/dev/block/mmcblk0p12`); requires root.
  - Netcat: transfers data over a network if the image cannot be saved to an SD card.
  - JTAG/Chip-off: advanced hardware methods. JTAG uses Test Access Ports (TAPs); Chip-off involves desoldering the NAND flash.
- *iOS*:
  - Physical: generally limited to legacy devices (pre-iPhone 5) due to hardware encryption.
  - Filesystem: requires jailbreaking (e.g., Electra, checkra1n) and connecting via SSH/iproxy to create a TAR archive of `/private/var/`.

### Screen Lock Bypassing & Access

| Method | Android | iOS |
|---|---|---|
| Developer Options | Tap Build Number 7 times to enable USB Debugging | N/A (requires passcode to establish Trust on iOS 11+) |
| ADB Bypass | Delete `gesture.key` or update `settings.db` (requires root) | Use Lockdown/Pairing certificates from a previously synced PC to trick the device |
| OS Exploits | UI crashing (long strings in emergency dialer for Android 5.x) | NAND Mirroring (soldering chips to bypass entry limits — up to iPhone 6s Plus) |
| Biometric Bypass | Smudge attacks (lighting up finger streaks) | 3D-printed masks (Face ID) or Play-Doh/dental molds (Touch ID) |
| Cloud Services | Android Device Manager or Samsung Find My Mobile | iCloud (reset backup password via "Reset All Settings" if device is unlocked) |

### Critical Forensic Artifact Locations

| Data Type | Android Location | iOS Location |
|---|---|---|
| Contacts | `contacts2.db` | `AddressBook.sqlitedb` (ABPerson/ABMultiValue tables) |
| SMS/MMS | `mmssms.db` | `sms.db` |
| Call Logs | `contacts2.db` ('calls' table); `calllog.db` (Nougat+) | `CallHistory.storedata` (ZCALLRECORD table) |
| Web History | `browser2.db` | `History.db` and `Bookmarks.db` |
| Notes | Package-specific (e.g., Google Keep) | `notes.sqlite` (ZNOTE/ZNOTEBODY tables) |
| Device Info | `/system/build.prop` | `info.plist` |
| App Artifacts | `shared_prefs` (XML) and internal databases | `.plist` files and databases in `/private/var/mobile/Containers/` |

### Interpretation: Timestamps

- **Android**: primarily uses Unix Epoch (seconds since Jan 1, 1970).
- **iOS**:
  - Mac Absolute Time: seconds since Jan 1, 2001 (difference from Unix is 978,307,200 seconds).
  - WebKit/Chrome Time: microseconds since Jan 1, 1601.

### Forensic Analysis & Recovery Tools

- **Android tools**: Autopsy (analyzing raw images), Scalpel (file carving), DiskDigger (on-device carving), and SQLite Browser.
- **iOS tools**: iBackup Viewer, iExplorer, Elcomsoft Phone Breaker (GPU-accelerated brute force), and Belkasoft (supports damaged backups).
- **Deleted data**: deleted records often persist in SQLite free blocks or unallocated space unless the database has been vacuumed.

**A way to think about it**: the Android filesystem is like a labeled filing cabinet — specific drawers (`/system`, `/data`) with everything organized in a way Linux commands can navigate directly. A locked drawer just needs a "root" key. iOS is closer to a curated library: Apple's own cataloging system (APFS) governs everything, and you can't walk into the back room (`/private/var`) without a "jailbreak" pass — though you can always ask for a "backup" copy of what you're permitted to see, and if you've lost access, a "Lockdown certificate" left behind by a trusted computer can get you back in.

## Part 3: Data Interpretation, Advanced Recovery, and Technical Nuances

### Specialized Timestamp Interpretation

- **Android**: primarily utilizes the Unix Epoch format, counting seconds elapsed since midnight on January 1, 1970.
- **iOS evolution**: historically used Unix, but transitioned to Mac Absolute Time with iOS 5, counting seconds from January 1, 2001. The difference between Unix and Mac time is exactly 978,307,200 seconds.
- **Browser-specific**: both platforms may use WebKit/Chrome time for web data, which counts microseconds since January 1, 1601.

### Deep Artifact Comparison (Database Tables)

| Data Type | Android Internal Detail | iOS Internal Detail |
|---|---|---|
| Call Logs | Stored in `contacts2.db` until Android 7.0; newer versions use `calllog.db` | Stored in `CallHistory.storedata` using the ZCALLRECORD table |
| Contacts | Found in `contacts2.db`; requires root to pull from `/data/data/` | Found in `AddressBook.sqlitedb` using ABPerson and ABMultiValue tables |
| Third-Party Apps | Directories are named by PackageName (e.g., `com.facebook.katana`) | Directories are named using a Universally Unique Identifier (UUID) |
| SMS/iMessage | Located in `mmssms.db` | Located in `sms.db`; unsent items are stored as `message.plist` in a Drafts folder |

### Advanced Hardware & Bypass Mechanics

- **Android physical bypass**: techniques like JTAG involve probing Test Access Ports (TAPs) on the CPU, while Chip-off involves physically desoldering the NAND flash chip. These work even if the device is powered off but are defeated by Full Disk Encryption (FDE).
- **iOS physical bypass**: physical acquisition is generally blocked by the Secure Enclave. Bypasses include NAND Mirroring (soldering chips to bypass passcode limits — up to iPhone 6s Plus) and using Lockdown/Pairing certificates from trusted computers.
- **Biometric spoofing**: iOS Touch ID can be tricked with fingerprint molds made of Play-Doh; Face ID can be bypassed using 3D-printed masks with 2D images and makeup.

### Deleted Data & File Carving

- **SQLite recovery**: both systems store deleted records in free blocks (within active pages) and unallocated blocks (pages no longer in use). Recovery is impossible if the database has been vacuumed or defragmented.
- **Signature carving**: reassembling files from raw bytes by searching for headers/footers (e.g., JPEG starts with `0xffd8` and ends with `0xffd9`).
- **Smart carving**: uses the specific fragmentation characteristics of filesystems (like APFS or EXT4) to recover non-contiguous data.

### Essential Metadata Files

- **Android identification**: the `build.prop` file in the `/system` folder contains the model, build version, and firmware details.
- **iOS backups**: a standard backup includes four vital files — `info.plist` (device details), `manifest.plist` (encryption status), `status.plist` (backup state), and `manifest.db` (SQLite list of all files).

**A way to think about it**: investigating Android can feel like archaeology in an open field — you can dig almost anywhere once you have root, though reaching the deepest layers still takes heavier tools like JTAG. Investigating iOS is closer to examining a modern smart safe: the digital display (a logical/iTunes backup) is easy enough to read, but getting into the safe itself means either a "Lockdown" keycard the owner left behind, or a more involved procedure (jailbreaking) to get past the internal sensors. Either way, timestamps are the detail that trips people up — the same event can read as 1601, 1970, or 2001 depending on which clock produced it.
