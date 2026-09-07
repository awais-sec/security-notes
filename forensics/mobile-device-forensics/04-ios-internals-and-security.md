# iOS Forensics: Understanding the Internals of iOS Devices

## Introduction

- 1.4 billion active Apple devices in 2019, with 900 million running iOS.
- iOS is the leading OS for tablets; Android leads for smartphones.
- A forensic examiner must understand iOS device internals to know: what data can be acquired, where data is stored, and how to access the data.
- This covers iPhone/iPad models, hardware, filesystems (HFS+ and APFS), and the iPhone OS.

## iPhone Models and Hardware

- First released in June 2007.
- Newer models (iPhone XR, XS, XS Max, 11, 11 Pro) are challenging for filesystem forensic acquisition.
- Physical recovery is generally not possible without jailbreaking.
- Logical acquisition is possible if the device is unlocked.

**Identifying the correct hardware model**

Essential for understanding acquisition possibilities (e.g., brute-forcing passcodes depends on model and iOS version).

- **Model number**: check the back of the device.
- **Apple's Knowledge Base**: support.apple.com.
- **Settings app**: Settings → General → About → Software Version & Model Name/Number.
- **Command-line tool**: `ideviceinfo` from the libimobiledevice library. Download binaries, run `ideviceinfo -s` in Command Prompt. Output includes `ProductType` (e.g., iPhone12,1) and `ProductVersion`.
- **GUI tools**: tools like iExplorer also display detailed device info.

**Importance of device details**

- Model: ensures tool and methodology compatibility.
- Storage size: prevents evidence drive space issues during acquisition.
- Network capabilities: allows for proper device isolation to prevent remote wipe/access.
- Encryption: newer models use full-disk encryption, requiring additional acquisition steps.
- iOS version: affects data storage locations. The original iOS version is as important as the current one.

**Understanding iPhone hardware**

A collection of modules, chips, and components from various manufacturers. Example (iPhone 11): A13 Bionic processor, 4 GB RAM, 64/128/256 GB storage, 6.1-inch Liquid Retina LCD, dual-lens 12 MP camera. Teardown guides are available at ifixit.com.

## iPad Models and Hardware

- Introduced January 2010.
- Similar to iPhones: various models (iPad Air 3, iPad Pro) with different features/storage.
- Same forensic challenges: not all versions support filesystem acquisition, and data storage locations change with iOS.
- Hardware is also a collection of components from different manufacturers. Teardown guides are available at ifixit.com.

## The HFS Plus and APFS Filesystems

### HFS Plus (Hierarchical File System Plus)

- Predecessor: HFS (1996). HFSX (case-sensitive variant) was originally used in iPhones.
- Designed for larger files. Uses 32-bit block addresses (vs. HFS's 16-bit).
- Uses journaling to prevent filesystem corruption.
- Key characteristics: efficient disk use, Unicode filenames, name forks, file compression, journaling, dynamic resizing/defragmentation, multi-OS boot support.

**HFS Plus volume structure**

- Reserved (1024 bytes): for bootloader.
- Volume Header: volume info (allocation block size, creation timestamp, special file metadata).
- Allocation File: bitmap tracking used/free allocation blocks (1 bit per block).
- Extents Overflow File: B-Tree. Records blocks for files >8 blocks; also records bad blocks.
- Catalog File: B-Tree. Contains the file/folder hierarchy.
- Attributes File: B-Tree. Contains inline, fork, and extension attribute records.
- Startup File: info for booting non-HFS Plus systems.
- Alternate Volume Header: backup of volume header for disk repair.
- Reserved (512 bytes): for Apple manufacturing.

### APFS (Apple File System)

- Default since iOS 10.3 (macOS since 10.13). Replaced HFS+.
- 64-bit, supports over 9 quintillion files.

**APFS main features**

- **Clones**: instant, space-efficient file/directory copies. Shares unmodified blocks.
- **Snapshots**: point-in-time, read-only filesystem instances.
- **Space sharing**: multiple filesystems share underlying free space.
- **Encryption**: three modes — no encryption, single-key encryption, or multi-key encryption (per-file keys for data plus a separate key for metadata). Uses AES-XTS or AES-CBC.
- **Crash protection**: copy-on-write metadata scheme.
- **Sparse files**: logical file size can exceed physical disk space.
- **Fast directory sizing**: quickly calculates directory space usage.

**APFS structure**

- Single Container holding one or more Volumes.
- Container Superblock: block size, block count, pointers to space manager, volume block IDs, pointer to block map B-Tree.
- Nodes: store entries; can be in B-Trees or standalone (fixed/flexible-size entries).
- Space Manager: manages allocated blocks in the container; tracks free blocks; points to the Allocation Info File.
- Allocation Info File: stores allocation file length, version, offset.
- B-Trees: manage nodes; contain root node offset.
- Volume Superblock: volume name, ID, timestamp.
- Allocation Files: simple bitmaps (no block header/type ID).

## Disk Layout

Two logical partitions:

1. **System Partition (Root/Firmware)**: contains OS and preloaded apps. Mounted as read-only (unless updating or jailbroken). Updated via iTunes firmware upgrades (formats entire partition). Small size (0.8–4 GB). Little evidentiary value unless jailbroken.
2. **User Data Partition**: contains all user-created data (music, contacts, third-party app data). Occupies most NAND memory. Mounted at `/private/var`. Primary source of evidence. Acquired as a `.tar` file during filesystem acquisition.

## The iPhone OS (iOS)

Derived from core OS X technologies. Unified OS for iPhone, iPod touch, iPad, Apple TV. Multi-touch interface.

**iOS architecture (four layers)**

1. **Cocoa Touch**: key frameworks for app visual interface. Provides basic app infrastructure, touch input, multitasking, high-level system services.
2. **Media**: graphics, audio, video frameworks for multimedia.
3. **Core Services**: fundamental system services (e.g., location, iCloud, social media).
4. **Core OS**: base layer on device hardware. Low-level functionalities: networking (BSD sockets), memory management, threading (POSIX), filesystem, external accessories, IPC.

## iOS Security

Layered security: hardware features against malware, OS features against unauthorized use.

**Key security features**

- **Passcodes, Touch ID, and Face ID**: restrict device access. Simple (4/6-digit) or complex passcodes. Touch ID (1 in 50,000 chance of stranger unlock). Face ID (1 in 1,000,000 chance).
- **Code Signing**: prevents unauthorized app installation. All executable code must be signed by an Apple-issued certificate.
- **Sandboxing**: restricts application access to files, network, hardware. Prevents apps from accessing each other's data.
- **Encryption**: entire filesystem encrypted with a key derived from a unique hardware key (stored in effaceable storage). Renders JTAG and chip-off methods ineffective (data dump is encrypted).
- **Data Protection**: protects data at rest. Uses device passcode plus hardware to generate a strong encryption key for disk data.
- **Address Space Layout Randomization (ASLR)**: randomizes memory locations to mitigate exploits (since iOS 4.3).
- **Privilege Separation**: two user roles — root (critical system processes) and mobile (user apps). Adheres to the Principle of Least Privilege (PoLP).
- **Stack-Smashing Protection**: uses stack canaries to detect buffer overflows.
- **Data Execution Prevention (DEP)**: processor distinguishes executable code from data to prevent code injection.
- **Data Wiping**: "Erase All Content and Settings" removes encryption keys, making data irrecoverable.
- **Activation Lock**: theft deterrent (since iOS 7). Tied to Find My iPhone. Requires Apple ID/password to disable Find My, erase, or reactivate.
- **The App Store**: centralized, curated app distribution. Apps reviewed by Apple (reduces malware risk, but not 100% secure — e.g., the XcodeGhost malware in 2015).

## Jailbreaking

- Process of removing Apple's restrictions via software/hardware exploits.
- Allows unsigned code to run and grants root access.
- **Purpose**: install unapproved apps, expand features.
- **Forensic use**: can aid acquisition.
- **Risks**: voids warranty, can "brick" the device, may prevent restoration.
- **Tools**: often install Cydia (unofficial app store). Popular tools: Pangu, TaiG, Electra, uncOver. Tool compatibility is version-specific.

## Summary

- The first step is always device identification.
- Model knowledge informs acquisition methods and possibilities.
- Do not disregard legacy devices, as they may appear in investigations.
- Understanding internals is crucial for a successful forensic examination.
