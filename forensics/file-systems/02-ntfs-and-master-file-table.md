# NTFS and the Master File Table

## NTFS (New Technology File System)

**Definition & history**: NTFS is a proprietary file system developed by Microsoft, introduced in 1993 with Windows NT 3.1. It was designed to overcome FAT's limitations by providing greater security, reliability, and support for large storage devices and files. It remains the default file system for all modern Windows versions.

**Purpose**: offers advanced file management and integrity features.

### Structure

Organizes data through key components:

- **Partition Boot Sector (PBS)**: located at the beginning, contains boot record, volume size, NTFS version, MFT starting location, BPB, and bootstrapping code.
- **Master File Table (MFT)**: a critical system file acting as a database, storing metadata about every file and directory on the volume.
- **System files**: hidden files for management, including `$MFT`, `$MFTMirr` (backup of MFT), `$LogFile` (transaction log for journaling), `$Bitmap` (tracks used/free clusters), `$BadClus` (list of bad clusters), and `$Secure` (security settings).
- **Data Area**: region where actual file content is stored, allocated in clusters.
- **Bitmap**: a file (`$Bitmap`) that tracks cluster availability (0 = free, 1 = allocated).
- **Log File (Journaling)**: maintains a log of changes to the file system (`$LogFile`) to ensure integrity during recovery.

### Features and Benefits

- **Journaling (transaction logging)**: prevents file system corruption by recording changes before applying them.
- **Security and permissions**: supports Access Control Lists (ACLs) for file/folder-level permissions.
- **File compression**: built-in support to save disk space.
- **Encryption (EFS)**: provides the Encrypting File System for data security.
- **Large file and volume support**: files up to 16 exabytes (theoretical), volumes up to 256 TB (practical).
- **Sparse file support**: efficiently manages large files with empty space.
- **Hard links and symbolic links**: multiple directory entries can point to the same file data.
- **Disk quotas**: limit disk space consumption for users.
- **Bad sector management**: detects and isolates bad sectors.

### How NTFS Works

**When creating a new file**:

- Searches `$Bitmap` for free clusters.
- Creates a new MFT entry for the file.
- Stores file metadata and (if small) content in the MFT.
- If large, data is stored in clusters with MFT pointers.
- Changes are first recorded in `$LogFile`.
- After successful writing, changes are committed and log entries cleared.

**When deleting a file**:

- Its MFT record is marked as deleted.
- Associated clusters in the `$Bitmap` are marked free.
- Actual data may remain until overwritten.

### Limitations

- **Limited cross-platform support**: full read-write support is limited on non-Windows systems without additional drivers.
- **Higher overhead**: uses more system resources than FAT.
- **Complexity**: more complex than simpler file systems.

## Master File Table (MFT) in NTFS

**Definition**: a critical component of NTFS that acts as a database, storing metadata about every file and directory on the volume.

**Purpose**: the primary role is to track all files and directories, with each having a corresponding entry containing metadata like size, location, and permissions. It's like a table of contents or catalog.

### Structure of an MFT Record

Each record (file record) describes a file or directory. It contains:

- **File metadata**: creation time, last access time, security permissions.
- **File attributes**: size, type, owner.
- **Pointers to file data**: location on disk (data runs).
- **Unique identifier** (File ID).

### MFT and File System Efficiency

- **Direct access**: allows quick reference to file metadata for opening or modifying files.
- **Small files optimization**: data for many small files is stored directly in the MFT record.
- **Large file management**: uses pointers in the MFT to reference data stored elsewhere on disk.

### MFT Size and Growth

Usually located at the beginning of the NTFS volume. Its size can grow dynamically as files are added or deleted. An MFT record is typically 1 KB. Deleted MFT records are marked free for reuse.

### Backup and Recovery

Vital for NTFS operation; MFT corruption can cause data loss. NTFS includes `$MFTMirr` (a backup copy) for recovery.

### MFT and File System Integrity

Closely linked; ensures accurate records. Journaling logs changes to the MFT for protection against data corruption.

### MFT and File Recovery

Tools scan the MFT to find lost or deleted files, as data may remain until overwritten even if the MFT record is marked free.

**Example**: for "document.txt", the MFT entry would contain a pointer to its data (or the data itself if small), metadata (name, creation time, permissions), data location, and a unique File ID.

## Memory Wastage After Deleting a File or Folder

**How deletion works**: when a file or folder is deleted in a file system like NTFS, its MFT entry is marked as free, making it no longer visible or accessible. However, the actual data blocks are also marked as free in the allocation table but not immediately reclaimed or overwritten. The data can still exist on disk until new data is written to those blocks.

**Is it really wastage?**: if new files are not immediately written, the space remains unused, which can be seen as temporary "memory wastage." However, this "wastage" isn't permanent — the space will eventually be reused when new data overwrites the deleted file's data blocks. The term "wastage" may be more applicable in fragmentation scenarios.

**Example**: deleting a 10 MB file frees its MFT entry and marks its 10 MB disk space as available. The 10 MB of data physically remains. If a new 5 MB file is created and written to a different part of the disk, the 10 MB previously used space remains unused until needed, which can be considered "wasted" until overwritten.
