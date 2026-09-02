# Computer Forensics Essentials

## What Computer Forensics Is About

Computer forensics is the process of identifying, preserving, collecting, analyzing, and presenting digital evidence in a way that's legally defensible. The end goal isn't just "find the data" : it's finding it *without changing it*, and being able to prove in court exactly how it was found, handled, and preserved along the way.

### The general phases (Important)

1. **Identification** : recognizing what devices, accounts, or data sources are relevant to the case
2. **Preservation** : making sure evidence isn't altered, whether by the suspect, by responders, or by the investigator's own tools
3. **Collection** : acquiring the data (see Data Acquisition below)
4. **Examination** : processing the acquired data to surface potentially relevant artifacts
5. **Analysis** : interpreting what those artifacts actually mean in the context of the case
6. **Presentation** : reporting findings clearly enough to hold up with a non-technical audience (a judge, a jury, a client)

### Core principles that run through everything below

- **Don't touch the original.** Every technique in this document : imaging, acquisition, string search : exists partly to avoid working directly on original evidence.
- **Order of volatility.** Some data disappears the moment a system is touched (RAM, network connections, running processes), while other data is comparatively stable (data on disk). This is why live acquisition and dead acquisition are treated as two separate categories with different urgency.
- **Chain of custody.** Every person who touches a piece of evidence, and every action taken on it, needs to be documented : from acquisition to the moment it's presented. Without this, evidence can be challenged or thrown out regardless of how sound the technical work was.
- **Reproducibility.** Someone else, given the same original evidence and the same documented process, should be able to arrive at the same findings. This is the whole reason forensic imaging is bit-by-bit rather than a selective copy.

Everything from here on : media analysis, string search, artifact analysis, acquisition, and imaging : is the practical toolkit for actually carrying out those phases and principles.

Note: These SOPs are important as these are the things which maintians the evidence integrity and make it admissible in court.

## Media Analysis

As you conduct a digital forensic investigation, artifacts you find may direct your focus to other locations. The goal of media analysis is to find relevant artifacts that either prove or disprove the allegations being investigated.

Timeline analysis feeds into several other types of analysis:

- **Network analysis** : analyzing log files, trace files, and communication content between users and their devices
- **Software analysis** : reverse-engineering malicious code or analyzing protection code for potential exploits
- **Media analysis** : analyzing forensic images of physical storage devices: hard drives, SSDs, thumb drives, optical discs, and mobile devices

### The progression of media analysis

1. **Disk** : physical storage devices (HDD, SSD, flash media)
2. **Volume** : a container comprising a single disk or multiple disks (a single disk can hold multiple volumes, or one volume can span multiple disks)
3. **File system** : operates within a volume's boundaries; tracks file allocation and cluster use
4. **Data unit** : the smallest allocation unit the file system uses (clusters on most systems, blocks on UNIX-based systems)
5. **Metadata** : data about data: modified/accessed/created timestamps and anything else the file system tracks about a file

## String Search

A string or byte search is used when you have a keyword list of specific terms to search for. Most commercial and open-source forensic tools support string searches across allocated, unallocated, and file slack space, using words, symbols, or letter strings as the search criteria. You'll generally want predefined keyword lists ready before starting an investigation.

### Keyword list categories

**1. Generic keyword list** : reused across every case, and can be further categorized by investigation subject (e.g. a separate list for fraud cases vs. one for illicit-image cases).

**2. Case-specific keyword list** : built for one specific investigation, based on participants, locations, and sometimes the slang used by those involved. Examples: usernames, email addresses, physical addresses, phone numbers, credit card numbers.

### Regular expressions

A regular expression uses character strings to build a search pattern, matching all instances that fit it:

| Symbol | Meaning | Example |
|---|---|---|
| `*` | Match the preceding character(s) zero or more times | `ca*t` matches `ct`, `cat`, `caat`, `caaat` |
| `#` | Match a digit (0–9) | |
| `\` | Treat the next character literally | `\.` matches a literal period |
| `^` | Match start of text | `^123` matches text starting with `123` |
| `$` | Match end of text | `123$` matches text ending with `123` |
| `+` | Repeat preceding character(s) one or more times | `ca+t` matches `cat`, `caat`, `caaat` |
| `{...}` | Repeat preceding character(s) a specific number of times | |
| `[...]` | Match a single character from the set | `[b,c,d]` matches `b`, `c`, or `d` |
| `[^...]` | Match any single character *not* in the set | `[^b,c,d]` matches anything except `b`, `c`, `d` |
| `[..-..]` | Match any character within a range | `[0-9]` matches any digit |
| `.` | Match any single character | |
| `?` | Preceding character may or may not be present | `.e01?` matches `.e0` plus any optional trailing value |
| `\|` | Match any one alternative separated by the pipe | `br(ead\|ake\|east)` matches `bread`, `brake`, or `breast` |

## Windows Artifacts

The goal of artifact analysis is to understand common OS artifacts you may encounter during an investigation. Seven areas covered:

1. Understanding user profiles
2. Understanding the Windows Registry
3. Determining account usage
4. Determining file knowledge
5. Identifying physical locations
6. Exploring program execution
7. Understanding USB/attached devices

### 1. Understanding user profiles

User profile storage location depends on Windows version:

- Windows XP, WinNT, Win2000: `C:\Documents and Settings\%UserName%`
- Windows Vista, 7, 8, 10: `C:\Users\%UserName%`

Eric Zimmerman's Registry Explorer is a common tool for analyzing user account information.

### 2. Understanding the Windows Registry

The Registry holds system settings and configuration. RegRipper and Eric Zimmerman's utilities are used to parse registry artifacts; registry hive files are exported for analysis.

### 3. Determining account usage

Event Viewer is used to examine log files for relevant events. Filtering results down helps focus on events specific to the investigation.

### 4. Determining file knowledge

Thumb-cache databases store thumbnail images from Windows Explorer. File knowledge determination involves exploring tables like `SystemIndex_0A` and `SystemIndex_PropertyStore`. Browsers also record file activity and history in their own specific locations.

### 5. Identifying physical locations

Recycle Bin analysis can recover data from unallocated clusters. Link file (`.lnk`) analysis using tools like Eric Zimmerman's LECmd provides metadata and file details.

### 6. Exploring program execution

ShimCache tracks compatibility issues with executed programs, and complements other artifact sources like the registry and event logs.

### 7. Understanding USB/attached devices

USB device information lives in the registry under `USBSTOR`. The `MountedDevices` key in the `SYSTEM` hive maps USB devices to drive letters on the system.

## Data Acquisition

Data acquisition is the use of established methods to extract Electronically Stored Information (ESI) from a suspect computer or storage media. Investigators must be able to verify the accuracy of acquired data, and the entire process needs to be auditable and admissible in court.

### Categories

**Live acquisition** : collecting data from a system that's powered ON, or volatile data from a live system. Helps establish the logical timeline of a security incident and the user's response. Captures system data and network data.

**Dead (static) acquisition** : collecting data from a system that's powered OFF, usually from storage devices (hard drives, USB drives, flash cards, smartphones). Static data examples: emails, documents, web activity, spreadsheets, unallocated drive space, deleted files.

### Thumb rules for data acquisition

1. Don't work on original evidence
2. Produce two or more copies of the original (one working copy, one for disclosure)
3. Use clean media to store the copies
4. Verify the integrity of the copies once created

### Types of acquisition

**Logical acquisition** : captures only selected files/file types at a logical level. Example: pulling specific records off a large RAID server.

**Sparse acquisition** : similar to logical acquisition, but also collects fragments of unallocated data, allowing recovery of deleted files. Used when a full drive inspection isn't required.

## Forensic Imaging

Forensic (disk) imaging is the process of creating a bit-by-bit copy of a digital storage device : capturing every piece of data, including deleted files and hidden sectors, so the original remains intact for examination and legal purposes.

### Why it matters

- **Preservation of evidence** : the original data isn't altered during analysis
- **Comprehensive data capture** : includes data not accessible through normal means
- **Legal integrity** : maintains chain of custody and provides a verifiable copy for court

### Tools

**Forensic Toolkit (FTK)** by AccessData is one of the leading tools in this space : a full investigation platform covering data imaging, evidence analysis, and report generation. Widely used across law enforcement, government, and private-sector forensics.

Key features: intuitive interface, imaging, indexing and searching, data carving, visualization, integration.

**Other tools:**
- Belkasoft Evidence Center
- Autopsy
- Recon Lab
- Paladin
- RegRipper and Eric Zimmerman's tools : for reading Windows registry data
