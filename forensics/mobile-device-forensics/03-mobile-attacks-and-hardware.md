# Mobile Attacks, Hardware, and OS Forensics

## Introduction to Mobile Device Forensics

- Mobile device forensics involves the recovery and examination of digital evidence from a mobile device's internal memory, SD cards, and SIM cards.
- Aims to trace perpetrators of crimes involving mobile phones.
- Devices are used for financial transactions, making them prime targets for malware and social engineering attacks.
- The field is continually evolving with new challenges from device features, encryption, apps, and cloud storage.
- Critical for criminal cases, corporate litigation, and private investigations.

## OWASP Top 10 Mobile Risks (2016)

A list of critical security risks for mobile applications.

1. **M1: Improper Platform Usage** — misuse of platform features or failure to use platform security controls (e.g., Android intents, Touch ID, Keychain).
2. **M2: Insecure Data Storage** — arises from assuming users/malware cannot access the device's file system. Caused by jailbreaking/rooting (bypassing encryption) and unintended data leakage (sensitive data placed in easily accessible locations).
3. **M3: Insecure Communication** — poor handshaking, weak SSL, cleartext communication. Can lead to account theft, phishing, and man-in-the-middle (MITM) attacks.
4. **M4: Insecure Authentication** — failing to identify the user, maintain user identity, or weaknesses in session management.
5. **M5: Insufficient Cryptography** — applying cryptography incorrectly or using weak algorithms, leading to unauthorized data retrieval.
6. **M6: Insecure Authorization** — failures in authorization decisions (e.g., on the client side), distinct from authentication failures.
7. **M7: Client Code Quality** — code-level implementation problems in the mobile client (e.g., buffer overflows, format string vulnerabilities). Can lead to foreign code execution or DoS on servers.
8. **M8: Code Tampering** — attackers modify the application code or resources post-delivery (e.g., binary patching, dynamic memory modification) to subvert its use.
9. **M9: Reverse Engineering** — analysis of the core binary to understand source code, algorithms, and uncover vulnerabilities or intellectual property using tools like IDA Pro, Hopper.
10. **M10: Extraneous Functionality** — developers accidentally include hidden backdoors or disabled security controls (e.g., passwords in comments, disabled 2FA) that attackers can discover and exploit.

## Mobile Attacks

Due to BYOD (Bring Your Own Device) policies, mobile devices are prime targets. Attacks can target the device, the network, or the data center/cloud.

### Point 01: The Device

Categories include pitching/framing/GK-lighting, man-in-the-middle, buffer overflow, and data caching.

**Browser-based attacks**

- **Phishing**: fake sites mimic trustworthy ones to steal personal info. More effective on mobiles due to small screens, short URLs, and limited warnings.
- **Framing**: using HTML iFrames to embed a malicious page within a legitimate one.
- **Clickjacking (UI redress attack)**: tricking users into clicking something different from what they perceive.
- **Man-in-the-mobile**: implanting malware to bypass OTP/password systems and relay information.

**Phone/SMS-based attacks**

- **Baseband attacks**: exploiting vulnerabilities in the phone's baseband processor (handles radio signals).
- **SMiShing (SMS phishing)**: sending deceptive SMS with malicious links/numbers to steal personal information.

**Application-based attacks**

- Sensitive data storage: apps with weak database security.
- No/weak encryption: susceptible to session hijacking.
- Improper SSL validation: allows attackers to circumvent data security.
- Configuration manipulation: exploiting external config files and libraries.
- Dynamic runtime injection: manipulating an app's runtime to bypass security, access privileged parts, or steal memory data.
- Unintended permissions: misconfigured apps granting unintended access.
- Escalated privileges: exploiting flaws to gain access to protected resources.
- Other methods: UI overlay/PIN stealing, third-party code, intent hijacking, ZIP directory traversal, clipboard data, URL schemes, GPS spoofing, weak local authentication, tampering, side-channel attacks, etc.

### Point 02: The Network

Categories include no/weak passcode, iOS jailbreaking, Android rooting, OS data caching, exposed passwords/data, carrier-loaded software, no/weak encryption, user-initiated code, zero-day exploits, device lockout, kernel driver vulnerabilities, and confused deputy attacks.

**Network-based attacks (Wi-Fi — weak/no encryption)**

- **Rogue access points**: illicit wireless APs installed to hijack legitimate connections.
- **Packet sniffing**: using tools (Wireshark) to capture clear-text data from traffic.
- **Man-in-the-middle (MITM)**: eavesdropping, intruding, and modifying data between two systems.
- **Session hijacking**: stealing valid session IDs for unauthorized access.
- **DNS poisoning**: substituting false IP addresses to redirect users to malicious sites.
- **SSLStrip**: a MITM attack that downgrades HTTPS connections to unencrypted HTTP.
- **Fake SSL certificates**: using fake certificates to intercept HTTPS traffic.
- Other methods: BGP hijacking, HTTP proxies.

### Point 03: The Data Center / Cloud

Categories include platform vulnerabilities, server misconfiguration, XSS, XSRF, weak input validation, brute-force attacks, cross-origin resource sharing, side-channel attacks, and hypervisor attacks.

**Web server-based attacks**

- **Platform vulnerabilities**: exploiting OS, server software (IIS), or application module flaws.
- **Server misconfiguration**: allowing unauthorized access to server resources.
- **Cross-Site Scripting (XSS)**: injecting malicious client-side scripts into web pages viewed by others.
- **Cross-Site Request Forgery (CSRF)**: forcing a user's browser to send malicious requests using their active session.
- **Weak input validation**: exploiting missing server-side validation to perform unauthorized actions (XSS, buffer overflow, injection).
- **Brute-force attacks**: trial-and-error to guess valid inputs.

**Database attacks**

- **SQL injection**: passing SQL commands through a web app to execute on a backend database.
- **Privilege escalation**: gaining high-level database access to steal data.
- **Data dumping**: forcing the database to dump sensitive records.
- **OS command execution**: injecting OS-level commands via a query to gain server access.

## Mobile Hardware and Forensics

- Forensics is highly dependent on the underlying hardware.
- No standard hardware architecture across devices (e.g., iPhone vs. Android).
- Investigators must use different tools and techniques for different phones.
- Knowledge of hardware architecture is essential, especially for chip-off forensics (physically removing the memory chip) or when a device is broken and cannot be accessed via data ports.

**Key mobile hardware components**

- **Antenna**: converts electromagnetic radiation to electric signals.
- **RF part**: includes up/down-converters, mixers, filters, and attenuators for frequency conversion.
- **ADC & DAC**: convert speech signals between analog and digital.
- **Baseband part**: contains a Digital Signal Processor (DSP) to process voice/data signals for transmission/reception (core for GSM, HSPA, LTE).
- **Application and CPU**: runs the OS and applications (audio, video, graphics, email, file transfer).
- **Memory unit (RAM, ROM)**: ROM is the internal storage.
- **Display**: various types (CSTN, TFT, LED, AMOLED, etc.).
- **Camera**: varying resolutions for front and rear cameras.
- **MIC (microphone)**: converts sound to electrical signals.
- **Speaker**: converts electrical signals to sound, coupled with an audio amplifier.
- **Connectivity**: Wi-Fi, Bluetooth, GPS, USB for data transfer and location services.

## Mobile OS and Forensics

- Focuses on the recovery, analysis, and presentation of data from mobile OSes in a legally admissible way.
- Challenging due to the wide variety and rapid evolution of OSes (Android, iOS, Windows Phone, BlackBerry OS).
- A fast-paced field requiring continuous learning.
- Investigators need knowledge of the underlying OS, architecture, file systems, and boot process to gain lower-level access.
- Remains a key component in criminal investigations, corporate litigation, and privacy protection.

## Mobile Forensics Challenges

1. **Varied nature**: multiple OSes, storage types, and data structures require learning various acquisition procedures and tools.
2. **Bypassing security**: difficulty bypassing PINs, passcodes, patterns, fingerprints, etc.
3. **Legal issues with cloud data**: acquiring cloud data is difficult due to legal constraints and requires specialized tools.
4. **Network isolation**: failure to isolate the device from networks can lead to remote evidence wiping.
5. **Jailbreaking/rooting risks**: can lead to data loss or device booting during physical acquisition.
6. **Tool compatibility**: difficulty finding correct tools for low-level access; incompatibility with certain devices/data types obstructs recovery.
7. **Anti-forensics techniques**: data wiping, forgery, hiding, and deletion (e.g., on multiple wrong password attempts).
8. **Data loss from updates**: frequent updates can cause data deletion or overwriting.
9. **Volatile data loss**: risk of losing volatile data if the device turns off during live capture.
10. **Privacy laws**: restricted access to devices or data due to privacy concerns and difficulty obtaining consent.
11. **Evolving technology**: makes investigations time-consuming and demands expensive, specialized tools and training.

## Android and iOS Architecture, Boot Process, and File Systems

- Understanding architecture, boot process, and file systems is fundamental for knowing where and how data is stored, processed, and accessed.
- Android and iOS have distinct architectures and boot processes due to unique designs and security models.
- Investigators need this knowledge to gain root access and aid the investigation.

**Mobile device architecture (block diagram overview)**

A mobile device is a coordinated system of hardware and software.

- **Input/Output**: display, keypad, ON/OFF switch, speaker, mic, camera.
- **Core processing & connectivity**: SIM, ROM, applications, baseband processing, CPU, RAM, USB, CODEC.
- **Signal processing & transmission**: DAC, RF part (frequency conversion, power amplification), ADC.
- **Power**: battery.
