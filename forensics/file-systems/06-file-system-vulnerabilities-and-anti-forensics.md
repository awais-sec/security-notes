# File System Vulnerabilities and Anti-Forensics

## File System Vulnerabilities

**Definition**: a weakness or flaw in the structure, configuration, or implementation of a file system that can be exploited by malicious users, malware, or unauthorized programs to gain access, manipulate data, or disrupt system operations.

**What is a file system?**: a method and data structure an operating system uses to manage files on a storage device; it organizes files into directories and tracks their locations. Examples include FAT32, NTFS, ext3/ext4, HFS+/APFS.

**How vulnerabilities occur**: when the system fails to properly protect file permissions, secure sensitive files, prevent unauthorized access, or handle corrupted files safely. These weaknesses can be exploited to read/write/delete critical data, install malware, or crash the system.

### Common Types of File System Vulnerabilities

- **Weak permissions**: users/programs have more access than needed (e.g., write access to system files).
- **Directory traversal**: attackers manipulate file paths (e.g., `../../etc/passwd`) to access restricted directories.
- **Buffer overflow**: exploiting file input fields to crash or hijack the system.
- **Insecure temporary files**: temp files can be accessed or modified by unauthorized users.
- **Symbolic link (symlink) attacks**: attackers trick the system into writing data to a different location via symlinks.
- **Improper file deletion**: deleted files can still be recovered if not securely wiped.
- **File metadata manipulation**: tampering with timestamps, ownership, etc.

### Solutions and Mitigations

| Vulnerability | Mitigation |
|---|---|
| Weak permissions | Apply least privilege, set strict permissions (`chmod`, `chown`), use ACLs, audit permissions |
| Directory traversal | Validate and sanitize input paths, use whitelisting, use secure APIs, avoid concatenating user input directly into file paths |
| Buffer overflow | Use bounds-checking functions, avoid unsafe functions (`gets()`, `strcpy()`), enable compiler protections (stack canaries, ASLR, DEP) |
| Insecure temporary files | Use secure temp file creation functions (`mkstemp()`), store in restricted directories, set proper permissions (0700) |
| Symlink attacks | Use the `O_NOFOLLOW` flag, validate paths, avoid writing to symlinks unless required, check file ownership |
| Improper file deletion | Use secure delete tools (`shred`, `srm`, `cipher`), overwrite files with random data; on SSDs, use TRIM with encryption |
| File metadata manipulation | Enable file integrity monitoring (Tripwire), restrict commands like `touch`/`chown`, log metadata changes, use digital signatures/hashes |

**Prevention and best practices**: use proper file permissions, apply security patches, validate input, secure temporary files, monitor file system activity, encrypt sensitive data, use secure file deletion tools.

## Techniques to Hide or Manipulate Data (Anti-Forensic Techniques)

**Definition of anti-forensics**: intentional actions taken by a criminal or attacker to hide, destroy, manipulate, or obscure digital evidence to avoid detection or make forensic analysis difficult.

### Basic Techniques

- **File renaming**: changing file extensions (e.g., `.exe` to `.txt`).
- **File attribute manipulation**: setting files to hidden, read-only, or system attributes.
- **Timestamp alteration**: using tools like `touch` to modify created, modified, or accessed times.
- **Simple obfuscation**: replacing characters with similar-looking ones in filenames or code.

### Intermediate Techniques

- **Data compression**: compressing data (ZIP, RAR, custom formats) to obfuscate contents.
- **Data encoding**: using Base64, Hex, or Unicode to encode malicious payloads.
- **Password-protected files**: encrypting or locking archives/files with passwords.
- **Alternate Data Streams (ADS)** (Windows): hiding data inside NTFS file streams not visible in standard file views.
- **File embedding**: hiding data in documents (macros) or images (EXIF metadata).

### Advanced Techniques

- **Steganography**: concealing data within multimedia files (images, audio, video) using tools like Steghide.
- **Encryption with stealth**: encrypting files using tools that leave minimal footprint or look like normal files.
- **Rootkits**: kernel-level tools that hide files, processes, or network activity from detection.
- **Filesystem tunneling**: exploiting Windows behavior where old file metadata is reused to obscure activities.
- **Virtual file systems / encrypted containers**: tools like VeraCrypt or TrueCrypt that create encrypted virtual disks to hide data.
- **Polymorphic and metamorphic code**: malware that changes its own code structure to avoid signature detection.
- **Memory-only execution (fileless malware)**: code runs directly in memory without touching the disk, making forensic tracking difficult.

### Benefits (From an Attacker/Privacy Perspective)

- Privacy protection: securing sensitive information.
- Digital rights management: preventing unauthorized use (watermarking).
- Anti-theft and secure communication: enabling confidential messaging.
- System efficiency: e.g., hidden temp files for optimization.
- Security testing (ethical): simulating attacks in red-team exercises.

### Disadvantages (Especially for Investigations)

- **Used by attackers**: malware often hides using these techniques.
- **Forensic challenges**: hidden/manipulated data makes investigations complex and time-consuming.
- **System misuse**: attackers misuse legitimate system features.
- **Data loss risk**: improper hiding/encryption may corrupt data.
- **Performance overhead**: advanced techniques can introduce system lag.
- **Legal issues**: hiding data may raise suspicion.

### Common Anti-Forensic Techniques (Summary)

- Data deletion (manual or special tools)
- Data overwriting (making recovery impossible)
- Encryption (unreadable without key)
- Steganography (hiding data in media)
- Timestamp manipulation
- File renaming/extension change
- Log tampering (deleting/editing logs)
- Use of live operating systems (avoiding local disk writes)

### How to Detect Anti-Forensic Techniques

- **File system analysis**: use forensic tools (FTK, Autopsy, EnCase) to detect deleted files, altered metadata, unusual behavior; compare timestamps.
- **Metadata examination**: analyze file metadata (ExifTool) for inconsistencies.
- **Unallocated space scanning**: search for traces of deleted/overwritten files using file carving.
- **Log file analysis**: review OS/application logs for tampering (missing entries, inconsistent gaps, signs of cleaners).
- **Memory (RAM) forensics**: examine RAM images for encrypted containers, passwords, stealth malware.
- **Steganography detection**: use steganalysis tools (StegDetect) to scan media files.
- **Keyword search and hash matching**: search for keywords, use hash databases to detect renamed/disguised files.
- **Behavioral analysis**: look for suspicious behavior (frequent encryption/secure deletion tool use, irregular usage patterns).

**Forensic tools commonly used for detection**: Autopsy/Sleuth Kit, FTK, Volatility, EnCase, ExifTool, X-Ways Forensics, StegDetect.
