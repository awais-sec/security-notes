# Incident Response and Management: Ransomware Case Studies

## 1. Case Study: SamSam Ransomware (2016–2018)

The SamSam group fundamentally changed the landscape by moving away from targeting regular users to focusing on human-operated attacks against entire organizations — pioneers of manual lateral movement.

- **Tactic** — moved laterally through networks to encrypt as many devices as possible, specifically targeting those with the most important data
- **Notable Targets** — healthcare, education, and cities; a 2018 attack on Atlanta, Georgia, cost the city approximately $2.7 million in recovery expenses

**Technical Execution:**
- **Foothold** — exploited public-facing applications (like JBOSS) or brute-forced RDP servers (RDP brute force and Mimikatz)
- **Escalation** — used hacking tools like Mimikatz to steal domain administrator credentials
- **Deployment** — scanned the network for hosts and used PsExec (a common dual-use tool) to deploy and run the ransomware

**Outcome:** the group earned roughly $6 million before two Iranian nationals were indicted by the FBI in late 2018.

**2018 Indictment:** the FBI charged two Iranian nationals, Faramarz Shahi Savandi and Mohammad Mehdi Shah Mansouri. Following this, the group ceased operations under the SamSam name.

## 2. Case Study: BitPaymer (2017)

Associated with the group "Evil Corp," BitPaymer introduced the trend of "Big Game Hunting" — targeting large organizations for massive ransoms.

- **Infection Method** — leveraged the Dridex trojan for initial access, which then loaded the PowerShell Empire post-exploitation framework, deployed via Group Policy
- **Example** — in August 2017, the group attacked hospitals in the NHS Lanarkshire board, demanding 53 BTC (approximately $230,000 at the time)

**Legal Outcome:** the FBI indicted Maksim Viktorovich Yakubets and Igor Olegovich Turashev for managing Dridex operations. There is a $5 million reward for Yakubets' apprehension.

## 3. Case Study: Ryuk Ransomware (2018–Present)

Ryuk took Big Game Hunting to new heights and is associated with the Wizard Spider group.

**The "Triple Threat" Chain:**
1. **Emotet** — acts as the initial "loader" or botnet that enters the system
2. **Trickbot** — once Emotet is inside, it downloads Trickbot, used for reconnaissance and stealing credentials
3. **Ryuk/Cobalt Strike** — Trickbot finally downloads a Cobalt Strike Beacon for manual control, eventually leading to the deployment of the Ryuk ransomware

**Financial Impact:** the group has successfully extracted at least $150 million from various organizations. More recently, the group has used BazarLoader and vishing.

**Legal Action:** in June 2021, Alla Witte (aka "Max") was indicted for her role in deploying the Trickbot trojan and ransomware. International law enforcement also took control of the Emotet botnet infrastructure in January 2021.

## 4. Case Study: Egregor

**TTP Summary:**
- **Initial Access** — trojans (Qakbot, Ursnif, IcedID) via phishing or unpatched VPNs
- **Reconnaissance** — AdFind, SharpHound, and SoftPerfect Network Scanner
- **Post-Exploitation** — Cobalt Strike for C2 and process injection
- **Lateral Movement** — RDP and PsExec for remote command execution
- **Exfiltration** — Rclone (renamed to `svchost.exe`), MEGA, and SendSpace
- **Defense Evasion** — disabling AV via Group Policy, PowerTool, or uninstalling SCEP
- **Notable Tool** — BITS, abused by actors like Egregor to download ransomware payloads

## 5. Case Study: Hive Ransomware — Technical Deep Dive

Technical reports (such as those from SentinelLabs) provide specific insights into the Hive ransomware group:

- **Coding Language** — Hive is written in Go (Golang), allowing it to run on multiple operating systems
- **Obfuscation** — uses UPX packing (Software Packing, T1027.002) to compress and hide its malicious code from simple detection
- **Self-Deletion** — once Hive finishes its task, it runs a batch file named `hive.bat` to delete the original malware file from the system to hide traces
- **Shadow Copy Removal** — executes another batch file, `shadow.bat`, which uses the command `vssadmin.exe delete shadows /all /quiet` to prevent the victim from easily restoring files (Inhibit System Recovery, T1490)
- **Service Termination** — before encrypting, it stops a long list of security and database services (Service Stop, T1489) to ensure no files are "in use" and can be fully encrypted

**Known Indicators of Compromise (SHA1 hashes identified in Hive attacks):**

```
67f0c8d81aefcfc5943b31d695972194ac15e9f2
edba1b73ddd0e32784ae21844c940d7850531b82
2877b32518445c09418849eb8fb913ed73d7b8fb
cd8e4372620930876c71ba0a24e2b0e17dcd87c9
eaa2e1e2cb6c7b6ec405ffdf204999853ebbd54a
0f9484948fdd1b05bad387b14b27dc702c2c09ed
e3e8e28a70cdfa2164ece51ff377879a5151abdf
9d336b8911c8ffd7cc809e31d5b53796bb0cc7bb
1cc80ad88a022c429f8285d871f48529c6484734
3b40dbdc418d2d5de5f552a054a32bfbac18c5cc
2f3273e5b6739b844fe33f7310476afb971956dd
7777771aec887896be773c32200515a50e08112a
5dbe3713b309e6ecc208e2a6c038aeb1762340d4
480db5652124d4dd199bc8e775539684a19f1f24
dc0ae41192272fda884a1a2589fe31d604d75af2
```

**Note:** while hashes are useful for research, they are less effective for detection because ransomware samples are often uniquely crafted for each specific attack, meaning the hashes will not match future samples (see the Pyramid of Pain in the fundamentals notes — hashes are the easiest indicator for an attacker to change).

## 6. Notable Ransomware Behaviors (Quick Reference)

| Strain | Notable Behavior |
|---|---|
| Crylock | Stops services, kills processes, and uses `vssadmin` and `wbadmin` to delete shadow copies and system state backups |
| REvil | Fingerprints the system, uses curve25519/Salsa20 encryption, and creates a unique registry key `HKLM\SOFTWARE\BlackLivesMatter` |
| LockBit | Uses AES-128 CBC, appends the `.lockbit` extension, and changes the desktop wallpaper to a red-and-white warning; StealBit is its custom exfiltration tool |

## 7. Double Extortion & Data Leak Sites (DLS)

The shift to human-operated ransomware brought the rise of "double extortion," where attackers steal data before encrypting it.

- **The Pioneers** — affiliates of the Maze ransomware group set the trend for this technique in 2019
- **The Mechanism** — if a victim refuses to pay for decryption, the attackers publish the stolen data on a Dedicated Leak Site (DLS)
- **Examples** — groups like DoppelPaymer maintain public DLS platforms to shame companies (e.g. Charlie Clark Nissan Brownsville) and announce successful breaches
- **Targeted Data** — exfiltrated data typically includes credit card information, Social Security numbers (SSNs), and Personally Identifiable Information (PII) such as names, emails, and phone numbers; also PHI (Protected Health Information) and NPIs (National Provider Identifiers) in healthcare-sector breaches

## 8. Post-Exploitation and Discovery Tools (Dual-Use)

Attackers and incident responders often use the same "dual-use" tools to navigate a network. While legitimate for administrators, they are frequently abused by threat actors during reconnaissance and lateral movement:

- **SoftPerfect Network Scanner & Advanced IP Scanner** — used to discover devices on a LAN, scan ports, and identify shared folders
- **Mimikatz** (and variants SafetyKatz, Invoke-Mimikatz) — the "gold standard" for dumping passwords, hashes, and Kerberos tickets from a system's memory
- **LaZagne** — an open-source tool used to retrieve saved passwords from various local applications like web browsers
- **AdFind / ADRecon** — used during the discovery phase to extract detailed information and artifacts from an Active Directory environment (e.g. domain trusts, users, and computers)
- **Cobalt Strike** — the most popular post-exploitation framework; specific IPs (e.g. `176.123.8.228`) have been identified as Cobalt Strike Beacons used by ransomware groups like Hive

---
*Source: coursework notes (AI-compiled study notes), Incident Response and Management module — merged and deduplicated from two overlapping source documents covering the same case studies at different levels of detail.*
