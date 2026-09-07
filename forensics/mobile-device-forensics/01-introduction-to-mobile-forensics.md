# Introduction to Mobile Device Forensics

## What is a Mobile Device?

- Cellular phones
- Tablet computers
- MP3 players
- e-Readers
- Wearable devices

## Why is Mobile Forensics Important?

- **Ubiquity**: the majority of people own and carry a cell phone at all times.
- **Data repository**: mobile devices are small computers that store an immense amount of sensitive and personal information (photos, messages, location data, documents).
- **Primary computing device**: many households now use mobile devices instead of traditional computers.
- **Use in crimes**: mobile devices are used to commit, or are involved in, various crimes including crimes against children, drug trafficking, harassment, terroristic threats, and murder, as well as civil wrongs.
- **Evolution of cell phones**: they have become smaller, lighter, less expensive, faster, and more power-efficient.

## Defining Mobile Forensics

- **Digital forensics**: a branch of forensic science encompassing the recovery and investigation of material found in digital devices, often in relation to computer crime.
- **Mobile forensics**: a branch of digital forensics relating to the recovery of digital evidence or data from a mobile device under forensically sound conditions.
- **Forensically sound**: a core principle requiring that the original evidence must not be altered. This is extremely challenging with mobile devices because:
  - Standard write protection often doesn't work.
  - Acquisition may require detaching a chip or installing a custom bootloader.
  - Any changes to the device must be carefully tested, documented, and justified.

## The Need for Mobile Forensics

- **Scale**: the number of mobile phone users was expected to pass 5 billion by 2020. Global mobile data traffic is growing exponentially (reaching exabytes per month).
- **Capability**: modern smartphones (e.g., iPhone, Samsung Galaxy) are high-performance computers with huge storage, used for communication, internet browsing, email, photography, GPS, and business tasks.
- **Evidentiary value**: data from phones is an invaluable source of evidence in criminal, civil, and high-profile cases. It's rare for a digital forensic investigation not to include a phone.
- **Real-world example**: mobile device call logs and GPS data were crucial in solving the 2010 attempted bombing in Times Square, New York.
- **Digital evidence**: defined as any and all digital data stored on, received by, or transmitted by an electronic device that can be used as evidence in a case.

## The Mobile Forensics Process — An Overview

The process consists of three main phases:

1. **Seizure**
2. **Acquisition**
3. **Examination/Analysis**

### Detailed Seizure Phase

- Ensure appropriate legal authority exists before seizing.
- Determine the device's make, model, and IMEI/MEID/serial number.
- Determine the goals of the examination.
- Wear gloves when handling evidence.

**Challenges**

- If the device is off, place it in a Faraday bag to prevent it from automatically powering on and connecting to a network.
- If the device is on, switching it off carries risks (e.g., data loss, triggering anti-theft features).

### Detailed Acquisition Phase

- Can be performed using multiple methods.
- Multiple attempts and tools may be necessary to acquire the maximum amount of data.
- The method chosen affects the amount of analysis required later.

### Detailed Examination/Analysis Phase

- Mobile phones are dynamic systems with many challenges.
- The sheer variety of manufacturers, models, and operating systems makes it impossible to have a single process or tool.
- Devices and technologies are continuously evolving.
- Special knowledge and skills are required from forensic experts.

## Challenges in Mobile Forensics

Mobile forensics presents unique challenges distinct from computer forensics:

1. **Hardware differences**: a vast and frequently updated market of mobile models with different sizes, hardware, and features.
2. **Mobile operating systems**: multiple OSes (iOS, Android, etc.) with numerous versions, unlike the PC market.
3. **Mobile platform security features**: built-in encryption and security mechanisms (e.g., Apple vs. FBI case) protect user data but hinder forensic acquisition.
4. **Preventing data modification**: a fundamental rule that is nearly impossible with mobiles. Simply turning on a device can alter data (e.g., background processes, alarm clocks).
5. **Anti-forensic techniques**: data hiding, obfuscation, forgery, and secure wiping used by suspects.
6. **Passcode recovery**: bypassing screen locks without damaging data is difficult and not universally reliable.
7. **Lack of resources**: the need for a wide array of tools, cables, batteries, and chargers for different devices.
8. **Dynamic nature of evidence**: evidence can be easily altered, even unintentionally (e.g., by opening an app).
9. **Accidental reset**: the risk of accidentally triggering a factory reset during examination.
10. **Device alteration**: a suspect with technical expertise may have modified the OS or data.
11. **Communication shielding**: devices must be isolated from all networks (cellular, Wi-Fi, Bluetooth) to prevent remote wiping or data alteration.
12. **Lack of availability of tools**: no single tool supports all devices; a combination is often needed.
13. **Malicious programs**: risk of the device containing malware that could spread to other devices.
14. **Legal issues**: crimes that cross geographical boundaries require knowledge of multi-jurisdictional laws.

## The Mobile Phone Evidence Extraction Process

While no single standard exists, a consistent process ensures evidence is well-documented and reliable. The phases are:

1. Intake
2. Identification
3. Preparation
4. Isolation
5. Processing
6. Verification
7. Documentation
8. Reporting
9. Archiving

### 1. The Evidence Intake Phase

- **Paperwork**: capture ownership, incident type, and data being sought.
- **Define objectives**: critical for clarifying examination goals.
- **Legal familiarity**: understand federal, state, and local laws regarding rights and seizure.
- **Legal authority**: government agents often need a search warrant (4th Amendment). Private parties have different rules.
- **Chain of custody**: a process that tracks the movement of evidence, documenting every person who handled it, along with dates/times and purposes (as defined by NIST).
- **Seizure best practice**: if the device is unlocked at seizure, try to disable the passcode to facilitate later access.

### 2. The Identification Phase

- **Legal authority**: document what legal authority exists for the examination and any limitations (e.g., defined by a warrant).
- **Data to be extracted**: define the scope of the examination to select appropriate tools and techniques.
- **Device details**: identify and document manufacturer, model, serial number; color, wallpaper, unique physical details (scratches); presence of hardware components (cameras, jack).
- **Data storage media**: process removable storage cards separately using traditional digital forensics, but also acquire them while in the device to preserve data linkages.
- **Other evidence**: mobile phones can be sources of fingerprints and biological evidence. Collect this first to avoid contamination, wearing gloves at all times.

### 3. The Preparation Phase

- Research the specific mobile phone model, OS, and version.
- Determine the appropriate methods and tools for acquisition and examination based on the device and the scope of the investigation.

### 4. The Isolation Phase

- **Goal**: isolate the device from all communication networks (cellular, Wi-Fi, Bluetooth) to prevent data alteration or remote wiping.
- **Methods**:
  - Faraday bags/tents/rooms: block all radio signals. This is the most common method.
  - Airplane mode: disables communication channels, but this may not be possible if the device is locked. Note: some devices allow Wi-Fi in airplane mode.

### 5. The Processing Phase

- **Tool selection**: choose tools based on price, ease of use, applicability, and forensic integrity (ability to package data in an unalterable format).
- **Acquisition methods (in order of preference)**:
  1. **Physical acquisition**: extracts the raw memory data. The device is usually powered off, resulting in the fewest changes. This is the preferred method.
  2. **File system acquisition**: attempt if physical acquisition fails or is not possible.
  3. **Logical acquisition**: should always be performed, as it contains parsed data and can provide pointers for analyzing the raw memory image.

### 6. The Verification Phase

Ensure the accuracy and integrity of the extracted data.

- **Compare to handset**: check if extracted data matches what is displayed on the device itself (use with caution to avoid altering the original).
- **Use multiple tools**: extract data with different tools and compare the results.
- **Hash values**: generate cryptographic hashes (e.g., MD5, SHA-1) for image files and extracted files. Any discrepancy in hash values must be explicable and documented.

### 7. The Documenting and Reporting Phase

- **Document everything**: the examiner must document every action taken during acquisition and examination.
- **Peer review**: results should be reviewed by another examiner to ensure completeness and accuracy.
- **Notes should include**: examination date/time, physical condition of the phone, photos, phone status, make/model, tools used, data found, and peer review notes.
- **The report**: findings must be presented clearly, concisely, and repeatably for court. Use timeline and link analysis tools to explain communication across multiple devices.

### 8. The Archiving Phase

- **Long-term preservation**: data must be retained in a usable format for the entire court process, which can last for years, including appeals.
- **Future-proofing**: as forensic methods advance, archived data can be re-examined to extract new information from old evidence.
