# Data Carving and Recovery

## Key Definitions

- **Data Carving**: The process of extracting files or data from a raw data source, where the file isn't tracked by a file system.
- **Data Recovery**: The process of restoring data that's been lost, corrupted, or deleted from storage media, using various techniques. Recovered through the file system.

## Header/Footer Method

1. A file's header is typically found in decimal, while the footer is in hex, so the footer needs converting to decimal.
2. Subtract the header offset from the footer offset; the result is the file size.
3. If the recovered image is blurred, that indicates some data is missing.
4. Opening the file in a hex editor's decoded section shows the file extension.
5. If the header is changed or damaged, expect disruption or data corruption in the recovered file.

## Image File Naming Conventions

- `ntfs-del.dd`: A forensic image of a deleted NTFS partition, containing deleted files and data.
- `ntfs.dd`: A bitwise forensic image of an NTFS partition, containing all data, including files, metadata, and unallocated space.
- `ext3.dd`: A forensic image of an ext3 file system (Linux), containing all data.
- `ext3-del.dd`: A forensic image of a deleted ext3 file system, containing deleted files and data that can be recovered.

## Alternate Data Streams (ADS)

When a file is hidden inside another file via ADS, the hidden file's header can still be found within the data.

## File Record Notation

Example: `r/r 78-128-2: unit_test.py` — `r/r` indicates a regular file, `78` is the serial key, `128` is the NTFS reference, and so on.

## Size vs. Size on Disk

- **Size**: The actual amount of data a file contains.
- **Size on disk**: The amount of physical disk space a file occupies, which may be larger due to file system overhead, fragmentation, and allocation unit size.

## For Exams

Things a screenshot/scenario will typically show:

- Which OS, which file system, whether a file is deleted or still exists, or if it's a sparse file / in slack space.
- Deleted normal vs. sparse: if a data group is entirely zeroes, that's a sparse file.

### Timeline and timestamp analysis

- Covers entering/closing times, but can't be relied on totally.
- Timestamps can be modified manually.
- System time itself can be changed (and will then differ from real time).
- On Linux, an inode's time can be changed.
- Always calculate the actual time rather than trusting the displayed value at face value.

### Case study: reconstructing real time from a faked system clock

```
Time of incident (real time):  2025-01-22 09:00 AM
Changed time (fake):           2025-01-23 03:00 AM

File access:      03:01 AM
File update:      03:20 AM
Exfiltrate time:   04:00 AM
```

Task: find the actual (real) time of the file access, update, and exfiltration, given the offset between the real incident time and the faked system time.

Note from original study material: worth sketching a clock to visualize this, and working the reasoning out on paper alongside it.
