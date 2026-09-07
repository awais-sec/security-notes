# FAT (File Allocation Table)

**Definition**: FAT is one of the oldest and simplest types of file systems used by operating systems to manage files on storage devices like hard drives, USB flash drives, and memory cards. It was originally developed by Microsoft in 1977.

**Purpose**: it's a crucial data structure in older file systems (e.g., MS-DOS, Windows 9x) primarily used for managing disk space and file organization. It acts as a map of the storage device, linking files to their respective clusters.

## Structure

- **Boot Sector**: contains important information about disk layout, file system type, and boot information, including the BIOS Parameter Block (BPB).
- **File Allocation Table (FAT)**: a special table that tracks which clusters are used by which files and which are free. Each entry corresponds to a cluster, and entries are linked to show chains of clusters for a file.
- **Root Directory**: stores information about files and directories at the root level, like file name, size, creation date, and starting cluster number.
- **Data Area**: the actual region where files and directory data are stored, divided into fixed-size units called clusters.

## How FAT Works

- **When a new file is saved**: the system finds free clusters in the FAT, allocates them, marks them as in use, and stores the starting cluster number in the file's directory entry. If the file is large, FAT links available clusters, and the last cluster is marked with an end-of-file (EOF) indicator.
- **When a file is read**: the system reads the starting cluster number from the directory and follows the chain in the FAT to read all related clusters until the EOF.
- **When a file is deleted**: the FAT entries are marked as free, allowing those clusters to be reused.

## Types of FAT

| Type | Max Partition Size | Max File Size | Typical Use |
|---|---|---|---|
| FAT12 | 32 MB | 32 MB | Floppy disks |
| FAT16 | 2 GB | 2 GB | Older DOS and Windows systems |
| FAT32 | 2 TB | 4 GB | Still widely used in flash drives and memory cards |

## Benefits

- **Simplicity**: very simple structure and operation.
- **Wide compatibility**: supported by almost all operating systems (Windows, macOS, Linux, embedded systems).
- **Low resource usage**: requires minimal memory and processing power, ideal for older computers and embedded systems.
- **Flexible media support**: works on various storage devices.
- **Good for small and removable storage**.

## Limitations

- No support for modern features like file permissions, journaling, or encryption.
- Limited maximum file size in FAT32 (4 GB).
- Slower performance on large disks or with many small files.
