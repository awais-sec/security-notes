# File Permissions and Journaling

## File Permissions

**Definition**: in a file system, permissions determine what actions users and groups can perform on files and directories (reading, writing, executing). They are a fundamental aspect of security.

**Purpose**: control access to data, protecting sensitive information from unauthorized access, modification, or deletion.

**Key actions**:

- **Read**: view contents of a file or list contents of a directory.
- **Write**: modify a file or create, delete, and rename files within a directory.
- **Execute**: run a program or script, or access and traverse a directory.

## Journaling

**Definition**: a mechanism where changes to the file system are first recorded in a log file (the journal) before being applied to the main file system structure. This enhances reliability and data integrity, especially during crashes or power failures. It logs what it's about to do before doing it.

### How It Works

- **Logging changes**: information about a change (e.g., creating/deleting/modifying a file) is first written to the journal.
- **Journal as a buffer**: acts as a temporary buffer, ensuring the operation is fully completed or can be rolled back.
- **Applying changes**: once recorded, the actual data and metadata are updated.
- **In case of crash**: on reboot, the file system reads the journal and can finish incomplete operations or roll them back.

### Benefits

- **Improved recovery**: journal can replay changes in progress at crash time to recover to a consistent state.
- **Reduced data loss**: minimizes corruption risk.
- **Faster recovery**: no need for a full consistency check (fsck) after a crash.
- **Enhanced data integrity**: operations are completed or rolled back completely.

### Why Journaling File Systems Are Needed

Without it, system crashes, power failures, or hardware failures could corrupt the file system if a write operation was incomplete, requiring a long file system check and potentially causing data loss.

### Types of Journaling

- **Write Ahead Logging (WAL)**: changes are first written to the journal before the actual disk.
- **Metadata Journaling**: only file system metadata (names, permissions, timestamps) is logged.
- **Full Data Journaling**: both metadata and actual file data are logged (slower, but safer).
- **Ordered Journaling**: metadata is journaled, and actual data is written before metadata changes are committed.

### Examples of Journaling File Systems

- **NTFS** (Windows): uses metadata journaling (plus transaction log).
- **ext3 and ext4** (Linux): journaling file systems.
- **ZFS** (Zettabyte File System): known for reliability, data integrity, snapshots, and replication.
- **ReiserFS** (Linux) and **XFS** (Linux, IRIX): also use metadata journaling.

### Real-Life Example

If a system crashes while saving a document, journaling allows the file system to either finish the transaction or roll it back on reboot, maintaining consistency.

### Forensic Importance

- Journals can hold valuable traces of activity (file creation, modification, deletion) even if the file is gone.
- Can reconstruct the sequence of events before a crash or shutdown.
- Can preserve evidence of tampering by malware or intruders.
- Tools like Autopsy and Sleuth Kit can analyze journals.
