# RAID (Redundant Array of Independent Disks)

**Definition**: a technology that combines multiple hard drives (or SSDs) into one unit to improve performance, increase storage reliability, or both. It uses a group of disks together.

**Main purposes**:

- Speed up data read/write operations.
- Protect data from disk failures (fault tolerance).
- Increase storage capacity.
- Improved performance.

## Basic Terminology

- **Striping**: splitting data across multiple disks for faster access.
- **Mirroring**: making exact copies of data on two or more disks.
- **Parity**: extra data calculated to recover lost data if a disk fails.

## Common RAID Levels

| Level | How It Works | Advantage | Disadvantage | Min. Disks | Use Case |
|---|---|---|---|---|---|
| RAID 0 (Striping) | Data is split (striped) across two or more disks | Very fast read and write speed | No data protection — if one disk fails, all data is lost | 2 | Gaming, graphic design, temporary high-speed storage |
| RAID 1 (Mirroring) | Data is exactly copied (mirrored) on two disks | High protection — if one disk fails, the other has full data | Storage capacity cut in half (50% loss) | 2 | Critical data storage (databases, personal backups) |
| RAID 5 (Striping with Parity) | Data and parity are spread across all disks | Good performance + protection; survives 1 disk failure | Slower write speeds than RAID 0 | 3 | File servers, small-to-medium business storage |
| RAID 6 (Double Parity) | Like RAID 5 but stores two sets of parity | Survives two disk failures | More complex and slower than RAID 5 | 4 | Large file servers, critical business storage |
| RAID 10 (1+0, Mirroring + Striping) | Combines RAID 1 and RAID 0 | High speed and high fault tolerance | Expensive (needs double the disks) | 4 | High-performance databases, large enterprise systems |

## Advantages of RAID

- Data protection and reliability.
- Faster data access.
- Bigger total storage.
- Continues to work even if a disk fails (depending on RAID level).

## Disadvantages of RAID

- Can be expensive (needs multiple disks).
- Complex setup and management.
- Some RAID types need special hardware (RAID controller).
- Not a substitute for backup (RAID protects against hardware failure, not accidental deletion or corruption).

## Hardware RAID vs. Software RAID

- **Hardware RAID**: a special hardware controller manages RAID independently from the CPU (e.g., RAID cards in servers).
- **Software RAID**: RAID is managed by the operating system (e.g., Windows Disk Management, Linux `mdadm`).

## Important Points to Remember

- **RAID is not a backup!** Always have separate backups.
- More disks generally mean better performance and safety.
- Choose the RAID level based on your specific needs (speed vs. safety vs. cost).
