# Practical Approaches and the Mobile Forensic Tool Leveling System

## Introduction and Mobile Operating Systems

- **Practical approaches**: the acquisition and examination method depends on the device type, operating system, and security settings. There is no single definitive procedure for all cases.
- **Importance of the OS**: the mobile OS is a major factor in data acquisition. It dictates access to the device (e.g., Android offers terminal-level access, while iOS does not). A comprehensive understanding is crucial for sound forensic decisions.

**Dominant mobile OSes**

- **Android**: the world's most widely used smartphone OS.
  - Linux-based, open-source platform from Google.
  - Chosen by manufacturers for being low-cost, customizable, and lightweight.
  - Its open nature encourages a large number of applications on Google Play.
- **iOS**: developed and distributed solely by Apple Inc. for iPhone, iPad, and iPod Touch.
  - Derived from macOS, making it a Unix-like OS.
  - Manages device hardware and provides technologies for native apps.
  - Applications are distributed through the closely monitored App Store.
- **Windows Phone** (distant third): a proprietary OS from Microsoft, successor to Windows Mobile, optimized for devices with limited storage.

## Mobile Forensic Tool Leveling System (Sam Brothers' Pyramid)

A classification system that categorizes forensic tools based on their methodology, with increasing technical complexity and risk from bottom to top.

**1. Manual Extraction (bottom tier)**

- Process: scrolling through the device's data via its keypad/touchscreen and photographing the screen.
- Pros: fast, easy, works on almost every phone.
- Cons: prone to human error, cannot recover deleted data, and may alter data (e.g., marking an unread SMS as read). Tools like Project-A-Phone can aid documentation.

**2. Logical Analysis**

- Process: connecting the device to a forensic workstation (via USB, Bluetooth, etc.). The computer sends commands, and the device's processor returns data from its memory.
- Pros: fast, easy, requires little training. Used by most common forensic tools.
- Cons: may write data to the device, altering evidence. Generally cannot access deleted data.

**3. Hex Dump (Physical Extraction)**

- Process: pushing unsigned code into the phone to dump its raw memory (a binary image) to a computer.
- Pros: inexpensive, provides more data, allows recovery of deleted files from unallocated space.
- Cons: requires technical expertise to analyze the raw binary image.

**4. Chip-Off**

- Process: physically removing the memory chip from the device and reading it with a chip reader or a second phone.
- Pros: preserves the exact state of memory. The only option for damaged devices with an intact chip. Works on screen-locked devices.
- Cons: destructive and expensive. Requires hardware-level knowledge (de-soldering). Improper procedure can destroy the chip and all data. The extracted raw data must be parsed and decoded. The JTAG (Joint Test Action Group) method is a common technique at this level.
- Recommendation: only attempt after other, less invasive methods have failed.

**5. Micro Read (top tier)**

- Process: using an electron microscope to view physical gates on the memory chip, translating their status to 0s and 1s to determine ASCII characters.
- Pros: the most fundamental level of data recovery.
- Cons: extremely time-consuming, costly, and requires extensive expertise. Only attempted in high-profile national security cases after all other methods are exhausted. No commercial tools are available for this.

## Data Acquisition Methods

**Physical Acquisition**

- A bit-by-bit copy of the physical storage (flash memory).
- Pros: captures all data, including deleted files and unallocated space. Similar to the approach in computer forensics.

**Logical Acquisition**

- Extracts logical storage objects (files, directories) using the device's API.
- Pros: easier for tools to organize and present data.
- Cons: only recovers existing files, not unallocated space. The device may be modified during the process.

**Manual Acquisition**

- Using the device's user interface to investigate and photograph content.
- Pros: easy to perform.
- Cons: high risk of human error and potential evidence deletion. Only acquires data visible on the screen.
- Best practice: should be the last option, after physical and logical. Can be used to validate findings from other methods.

## Potential Evidence Stored on Mobile Phones

Data can be found in phone memory, SIM cards, and external storage. Common evidence includes (often with timestamps):

- Address book: contact names, numbers, email addresses.
- Call history: dialed, received, missed calls with duration.
- SMS/MMS: sent and received text and multimedia messages.
- Email: sent, drafted, and received emails.
- Web browser history: visited websites.
- Photos/videos/music: files captured, downloaded, or transferred.
- Documents: files created, downloaded, or transferred.
- Calendar: entries and appointments.
- Network communication: GPS locations.
- Maps: searches, directions, and visited places.
- Social networking data: data from apps like Facebook, WhatsApp, etc.
- Deleted data: information deleted from the phone, often recoverable via physical acquisition.

## Examination and Analysis

- **Goal**: to uncover and probe data present on the device, separating relevant information for the case.
- **Process**: starts with a copy of the acquired evidence, often using third-party tools to automatically parse the memory dump.
- **Targeted analysis**: understanding the case is crucial (e.g., a child pornography case requires a focus on image analysis).
- **Tool proficiency and limitations**:
  - Forensic examiners must understand how their tools work to use them effectively.
  - They must also recognize tool limitations (e.g., programming flaws, inability to parse certain data) and be prepared to use alternate tools or methods.
  - Analysts must compensate for these limitations to achieve the best results.

## Rules of Evidence

For evidence to be useful and admissible in court, it must follow five general rules:

1. **Admissible**: the evidence must be gathered and preserved in a way that makes it usable in court. Evidence gathered illegally is typically inadmissible.
2. **Authentic**: the evidence must be relevant to the incident. The examiner must be accountable for its origin.
3. **Complete**: the evidence must tell the whole story, not just one perspective. Incomplete evidence can be more damaging than no evidence.
4. **Reliable**: the evidence must be dependable. The tools and methodology used must not cast doubt on its authenticity. The process should be reproducible.
5. **Believable**: the forensic examiner must be able to clearly and concisely explain the processes used and how evidence integrity was preserved. The evidence must be understandable and credible to a jury.

## Good Forensic Practices

**Securing the evidence**

- Isolate the device: use Faraday bags or other RF shielding to prevent remote wipes and network communication.
- Preserve physical evidence: handle devices with gloves; collect peripherals, cables, and accessories.
- Connected computers: if connected to a PC, capture the computer's memory before disconnecting the device, as it may contain valuable data.

**Preserving the evidence**

- Work on copies: never work on the original evidence. Create a read-only master copy and perform all examination on duplicates.
- Use forensic hashes: create hash values (e.g., MD5, SHA-1) for the original and copies to verify integrity. Any hash changes must be documented and explicable.
- Minimize interaction: only perform necessary tasks on the device to avoid altering data.

**Documenting the evidence and changes**

- Photograph everything: photograph the device and its environment without touching it.
- Detail everything: document all methods, tools, and changes made during acquisition and examination (e.g., power cycling, tool-slicing of disk images).
- Reproducibility: notes must be so detailed that another examiner can reproduce the entire process. Non-reproducible work may be ruled inadmissible.

**Reporting**

- A detailed summary of all steps, conclusions, and inferences.
- Report contents should include: reporting agency and case identifier; investigator and submitter identity; device details (make, model, serial number); tools and steps used in the examination; chain of custody; findings and recovered evidence (messages, logs, deleted data, etc.); images from the examination; analysis information and a conclusion.
